## OpenKruise

[OpenKruise](https://github.com/openkruise/kruise) 是阿里云开源的大规模应用自动化管理引擎，在功能上对标了 Kubernetes 原生的 Deployment / StatefulSet 等控制器，但 OpenKruise 提供了更多的增强功能如：优雅原地升级、发布优先级 / 打散策略、多可用区 workload 抽象管理、统一 sidecar 容器注入管理等，都是经历了阿里巴巴超大规模应用场景打磨出的核心能力。这些 feature 帮助我们应对更加多样化的部署环境和需求、为集群维护者和应用开发者带来更加灵活的部署发布组合策略。

目前，Kruise 提供了以下 workload 控制器：

- CloneSet: 提供了更加高效、确定可控的应用管理和部署能力，支持优雅原地升级、指定删除、发布顺序可配置、并行 / 灰度发布等丰富的策略，可以满足更多样化的应用场景。
- Advanced StatefulSet: 基于原生 StatefulSet 之上的增强版本，默认行为与原生完全一致，在此之外提供了原地升级、并行发布（最大不可用）、发布暂停等功能。
- SidecarSet: 对 sidecar 容器做统一管理，在满足 selector 条件的 Pod 中注入指定的 sidecar 容器。
- UnitedDeployment: 通过多个 subset workload 将应用部署到多个可用区。
- BroadcastJob: 配置一个 job，在集群中所有满足条件的 Node 上都跑一个 Pod 任务。
- Advanced DaemonSet: 基于原生 DaemonSet 之上的增强版本，默认行为与原生一致，在此之外提供了灰度分批、按 Node label 选择、暂停、热升级等发布策略。

[是的，不修改源码可以直接原地升级 Pod！](https://my.oschina.net/u/4148359/blog/4702121)

## Deployment API 版本对照表

| Kubernetes 版本 | Deployment 版本    |
| --------------- | ------------------ |
| v1.5-v1.15      | extensions/v1beta1 |
| v1.7-v1.15      | apps/v1beta1       |
| v1.8-v1.15      | apps/v1beta2       |
| v1.9+           | apps/v1            |

## Deployment yaml

Deployment yaml 文件包含四个部分：

- apiVersion: 表示版本
- kind: 表示资源
- metadata: 表示元信息
- spec: 资源规范字段

Deployment yaml 名词解释：

```yaml
apiVersion: apps/v1 # 指定api版本，此值必须在kubectl api-versions中
kind: Deployment # 指定创建资源的角色/类型
metadata: # 资源的元数据/属性
  name: demo # 资源的名字，在同一个namespace中必须唯一
  namespace: default # 部署在哪个namespace中
  labels: # 设定资源的标签
    app: demo
    version: stable
spec: # 资源规范字段
  replicas: 1 # 声明副本数目
  revisionHistoryLimit: 3 # 保留历史版本
  selector: # 选择器
    matchLabels: # 匹配标签
      app: demo
      version: stable
  strategy: # 策略
    rollingUpdate: # 滚动更新
      maxSurge: 30% # 最大额外可以存在的副本数，可以为百分比，也可以为整数
      maxUnavailable: 30% # 示在更新过程中能够进入不可用状态的 Pod 的最大值，可以为百分比，也可以为整数
    type: RollingUpdate # 滚动更新策略
  template: # 模版
    metadata: # 资源的元数据/属性
      annotations: # 自定义注解列表
        sidecar.istio.io/inject: "false" # 自定义注解名字
      labels: # 设定资源的标签
        app: demo
        version: stable
    spec: # 资源规范字段
      containers:
        - name: demo # 容器的名字
          image: demo:v1 # 容器使用的镜像地址
          imagePullPolicy:
            IfNotPresent # 每次Pod启动拉取镜像策略，三个选择 Always、Never、IfNotPresent
            # Always，每次都检查；Never，每次都不检查（不管本地是否有）；IfNotPresent，如果本地有就不检查，如果没有就拉取
          resources: # 资源管理
            limits: # 最大使用
              cpu: 300m # CPU，1核心 = 1000m
              memory: 500Mi # 内存，1G = 1024Mi
            requests: # 容器运行时，最低资源需求，也就是说最少需要多少资源容器才能正常运行
              cpu: 100m
              memory: 100Mi
          livenessProbe: # pod 内部健康检查的设置
            httpGet: # 通过httpget检查健康，返回200-399之间，则认为容器正常
              path: /healthCheck # URI地址
              port: 8080 # 端口
              scheme: HTTP # 协议
              # host: 127.0.0.1 # 主机地址
            initialDelaySeconds: 30 # 表明第一次检测在容器启动后多长时间后开始
            timeoutSeconds: 5 # 检测的超时时间
            periodSeconds: 30 # 检查间隔时间
            successThreshold: 1 # 成功门槛
            failureThreshold: 5 # 失败门槛，连接失败5次，pod杀掉，重启一个新的pod
          readinessProbe: # Pod 准备服务健康检查设置
            httpGet:
              path: /healthCheck
              port: 8080
              scheme: HTTP
            initialDelaySeconds: 30
            timeoutSeconds: 5
            periodSeconds: 10
            successThreshold: 1
            failureThreshold: 5
          #也可以用这种方法
          #exec: 执行命令的方法进行监测，如果其退出码不为0，则认为容器正常
          #  command:
          #    - cat
          #    - /tmp/health
          #也可以用这种方法
          #tcpSocket: # 通过tcpSocket检查健康
          #  port: number
          ports:
            - name: http # 名称
              containerPort: 8080 # 容器开发对外的端口
              protocol: TCP # 协议
      imagePullSecrets: # 镜像仓库拉取密钥
        - name: harbor-certification
      affinity: # 亲和性调试
        nodeAffinity: # 节点亲和力
          requiredDuringSchedulingIgnoredDuringExecution: # pod 必须部署到满足条件的节点上
            nodeSelectorTerms: # 节点满足任何一个条件就可以
              - matchExpressions: # 有多个选项，则只有同时满足这些逻辑选项的节点才能运行 pod
                  - key: beta.kubernetes.io/arch
                    operator: In
                    values:
                      - amd64
```

Service yaml 名词解释：

```yaml
apiVersion: v1 # 指定api版本，此值必须在kubectl api-versions中
kind: Service # 指定创建资源的角色/类型
metadata: # 资源的元数据/属性
  name: demo # 资源的名字，在同一个namespace中必须唯一
  namespace: default # 部署在哪个namespace中
  labels: # 设定资源的标签
    app: demo
spec: # 资源规范字段
  type: ClusterIP # ClusterIP 类型
  ports:
    - port: 8080 # service 端口
      targetPort: http # 容器暴露的端口
      protocol: TCP # 协议
      name: http # 端口名称
  selector: # 选择器
    app: demo
```
