<!--toc-->
[TOC]
[awesome-go](https://github.com/avelino/awesome-go)
## x
golang.org/x/time/rate
该限流器是基于Token Bucket(令牌桶)实现的。


## 分析/debug
### pprof
[Profiling](./pprof.md)
### visualgdb 工具
https://visualgdb.com/gdbreference/commands/x

### goweight 分析模块大小
```
$ cd current-project
$ goweight
```

### godebug:一个跨平台的Go程序调试工具
https://github.com/mailgun/godebug  已过时

### godebug:delve
https://github.com/go-delve/delve

### yaegi  go解释器
https://github.com/containous/yaegi
可以提供交互环境

https://github.com/topxeq/gotx

### 热更新
https://github.com/cosmtrek/air


https://github.com/topics/live-reload?l=go

### gops 分析机器上运行了哪些go进程
go get -u github.com/google/gops

```
C:\Users\35084>gops tree
...
├── 16712
│   └── 5988 (gops.exe) {go1.14.1}
├── 4728
│   ├── 16028 (com.docker.backend.exe) {go1.12.16}
│   └── 3708 (com.docker.proxy.exe) {go1.12.16}
└── 5172
    └── 12080 (gopls.exe) {go1.14.1}


C:\Users\35084>gops
18256 16712 gops.exe                go1.14.1  D:\go\bin\gops.exe
16028 4728  com.docker.backend.exe  go1.12.16 C:\Program Files\Docker\Docker\resources\com.docker.backend.exe
12080 5172  gopls.exe               go1.14.1  D:\go\bin\gopls.exe
3708  4728  com.docker.proxy.exe    go1.12.16 C:\Program Files\Docker\Docker\resources\com.docker.proxy.exe

C:\Users\35084>gops 3708
parent PID:     4728
threads:        12
memory usage:   0.058%
cpu usage:      0.001%
username:       DESKTOP-PK520IC\35084
cmd+args:       "com.docker.proxy.exe"  -dockerExe "C:\Program Files\Docker\Docker\resources\bin\docker.exe"  -host-names host.docker.internal,docker.for.win.host.internal,docker.for.win.localhost -gateway-names gateway.docker.internal,docker.for.win.gateway.internal,docker.for.win.http.internal -vm-names vm.docker.internal,docker-for-desktop,docker-desktop,kubernetes.docker.internal -host-ip 192.168.65.2 -gateway-ip 192.168.65.1 -vm-ip 192.168.65.3 -pki "C:\ProgramData\DockerDesktop\pki" -inject-hosts=True
elapsed time:   02:45:26
local/remote:   127.0.0.1:33499 <-> 0.0.0.0:0 (LISTEN)
local/remote:   127.0.0.1:53974 <-> :0 ()
```

## 流量回放工具

### Goreplay
https://github.com/buger/goreplay/

[流量回放工具之 Goreplay 安装及初级使用](https://juejin.cn/post/6999586008698208263)

### Sharingan
https://kgithub.com/didi/sharingan

### 其它
- [tcpcopy](https://kgithub.com/session-replay-tools/tcpcopy)

## 分析网站使用的技术
### Wappalyzer
分析网站使用的技术
https://github.com/rverton/webanalyze/

https://github.com/wappalyzer/wappalyzer

### WhatRuns 

## 可视化
### Go Callvis
可视化 Go 程序的调用图
https://github.com/TrueFurby/go-callvis

## json
github.com/liamylian/json-hashids

序列化自动加密

## orm
facebook开源的新的go语言orm模块，An entity framework for Go
Simple, yet powerful ORM for modeling and querying data.
https://github.com/facebookincubator/ent


## DI


### Google wire 3.3k
https://github.com/google/wire

Wire 可以生成 Go 源码并在编译期完成依赖注入。它不需要反射机制或 [Service Locators](https://en.wikipedia.org/wiki/Service_locator_pattern)

好处：
1. 方便 debug，若有依赖缺失编译时会报错
2. 因为不需要 Service Locators， 所以对命名没有特殊要求
3. 避免依赖膨胀。生成的代码只包含被依赖的代码，而运行时依赖注入则无法作到这一点
4. 依赖关系静态存于源码之中， 便于工具分析与可视化

[Compile-time Dependency Injection With Go Cloud's Wire](https://blog.golang.org/wire)


[一文读懂 Go官方的 Wire](https://mp.weixin.qq.com/s/ZQKi9O7DRJ3qGWhDL9aTVg)

### Uber dig 1.3k
运行时依赖注入
https://github.com/uber-go/dig

### Facebook inject 1.2k(归档了，不更新)
运行时依赖注入
https://github.com/facebookarchive/inject

## spinal-case(脊柱) or snake_case(蛇形) or CamelCase(驼峰式) or KebabCase(短横线) or PascalCase(帕斯卡命名法) or PascalSnakeCase
https://github.com/iancoleman/strcase

## GUI


Cross platform GUI in Go based on Material Design https://fyne.io/
https://github.com/fyne-io/fyne


请注意，默认情况下，Windows应用程序是从命令提示符加载的，这意味着，如果单击图标，则可能会看到命令窗口。 要解决此问题，请在运行或构建命令中添加参数-ldflags -H = windowsgui。



Prerequisites
https://fyne.io/develop/
Windows
1. Download Go from the download page and follow instructions
2. Install one of the available C compilers for windows, the following are tested with Go and Fyne:
    - MSYS2 with MingW-w64 - msys2.org
    - TDM-GCC - tdm-gcc.tdragon.net
    - Cygwin - cygwin.com
3. In Windows your graphics driver will already be installed, but it is recommended to ensure they are up to date.


## eval
github.com/Knetic/govaluate

## 限流熔断
https://github.com/alibaba/sentinel-golang

https://github.com/afex/hystrix-go

golang 提供了拓展库(golang.org/x/time/rate)提供了限流器组件

## 微服务
https://github.com/douyu/jupiter

## book
《Go语法树入门》(开源免费图书/Go语言进阶/掌握抽象语法树/Go语言AST/LLVM/LLIR)
https://github.com/chai2010/go-ast-book


## log
https://github.com/grafana/loki


## 流媒体
monibuca  丰富的内置插件提供了流媒体服务器的常见功能，例如rtmp server、http-flv、视频录制、QoS等
https://github.com/Monibuca 100star


Darwin Streaming Server 没有维护了？
https://github.com/macosforge/dss


高性能开源RTSP流媒体服务器，基于go语言研发，维护和优化：RTSP推模式转发、RTSP拉模式转发、录像、检索、回放、关键帧缓存、秒开画面、RESTful接口、WEB后台管理、分布式负载均衡
https://github.com/EasyDarwin/EasyDarwin   4k 

https://github.com/pion/ion
https://github.com/pion/webrtc

IOT 平台？
https://github.com/nats-io/nats-streaming-server

## Chrome DevTools Protocol 
https://github.com/chromedp/chromedp
https://www.toutiao.com/i6843024797073408515


## 代码质量检测工具
https://github.com/mgechev/revive

## tests
它是一个 Golang 命令行工具，它根据目标源文件的功能和方法签名生成表驱动测试。
https://github.com/cweill/gotests

## grpc
### grpcui
他是一个 gRPC 的 Web 页面调试工具，提供交互式的调试界面。
https://github.com/fullstorydev/grpcui

grpcui -plaintext 127.0.0.1:9901
127.0.0.1:9901 是grpc server的地址

### bloomrpc
GUI Client for GRPC Services
https://github.com/uw-labs/bloomrpc

### insomnia
GraphQL、REST和gRPC的开源、跨平台API客户端。
https://github.com/Kong/insomnia

## dingtalk
```
package dingtalk
​
import (
    "bytes"
    "encoding/json"
    "errors"
    "io/ioutil"
    "net/http"
    "strconv"
    "time"
)
​
// SendMessage 发送钉钉机器人消息
func SendMessage(url, message string, ats ...string) (respContent string, err error) {
    c := &http.Client{
        Timeout: time.Second * 30,
    }
    data := map[string]interface{}{
        "msgtype": "text",
        "text":    map[string]string{"content": message},
    }
    if len(ats) != 0 {
        isAtAll := false
        atMobiles := []string{}
        for i := range ats {
            if ats[i] == "all" {
                isAtAll = true
            } else {
                atMobiles = append(atMobiles, ats[i])
            }
        }
        data["at"] = map[string]interface{}{
            "isAtAll":   isAtAll,
            "atMobiles": atMobiles,
        }
    }
    b, _ := json.Marshal(data)
    resp, err := c.Post(url, "application/json", bytes.NewReader(b))
    if err != nil {
        return "post请求失败", err
    }
    defer resp.Body.Close()
    body, _ := ioutil.ReadAll(resp.Body)
    if resp.StatusCode == 200 {
        return string(body), nil
    }
    return string(body), errors.New(strconv.Itoa(resp.StatusCode))
}

package main
​
import (
    "fmt"
    "dingtalk"
)
​
func main() {
    result, err := dingtalk.SendMessage("https://oapi.dingtalk.com/robot/send?access_token=XXXXXX", "测试消息通知", "all")
    if err != nil {
        fmt.Println("发送失败", result)
        return
    }
    fmt.Println("发送成功", result)
}
```

## WebAssembly
从Go到JavaScript的编译器，用于在浏览器中运行Go代码
https://github.com/gopherjs/gopherjs


https://github.com/hexops/vecty

## js
### esbuild
https://github.com/evanw/esbuild
这是一个 JavaScript 打包和压缩程序。它用于打包 JavaScript 和 TypeScript 代码以在网络上分发。
我目前有两个基准测试用于衡量 esbuild 的性能。对于这些基准测试，esbuild 比我测试的其他 JavaScript 打包程序 快至少 100 倍 ：https://docs.breword.com/evanw-esbuild/

类似工具：
- Webpack ， Rollup 或 Parcel 用于打包
- Babel 或 TypeScript 用于转译
- Terser 或 UglifyJS 用于代码压缩

使用者：
- Vite
- Snowpack
> Vite，snowpack使用了esbuild

> esbuild 不可能替代 webpack、parcel 等构建工具

## git

### git sql
https://github.com/augmentable-dev/askgit

也可以跑docker
docker run -v F:/github/openjw/openself:/repo:ro augmentable/askgit "SELECT * FROM commits"

## Markdown
将 markdown 中的 go 代码块进行格式化。
https://github.com/po3rin/gofmtmd

https://github.com/JohannesKaufmann/html-to-markdown

## 嵌入文件

[golang将静态资源文件打包进二进制文件](https://zhuanlan.zhihu.com/p/351931501)
[golang1.16内嵌静态资源指南](https://www.cnblogs.com/apocelipes/p/13907858.html)

https://github.com/golang/proposal/blob/master/design/draft-embed.md#background

- github.com/alecthomas/gobundle
- github.com/GeertJohan/go.rice
- github.com/go-playground/statics
- github.com/gobuffalo/packr
- github.com/knadh/stuffbin
- github.com/mjibson/esc
- github.com/omeid/go-resources
- github.com/phogolabs/parcello
- github.com/pyros2097/go-embed
- github.com/rakyll/statik
- github.com/shurcooL/vfsgen
- github.com/UnnoTed/fileb0x
- github.com/wlbr/templify
- perkeep.org/pkg/fileembed

2020 年 10 月 30 日，Russ Cox 提交了最终的实现：[cmd/go: add //go:embed support](cmd/go: add //go:embed support)，意味着你在 tip 版本可以试用该功能了。Go1.16 版本会包含该功能。
Embed 设计提案：https://github.com/golang/proposal/blob/master/design/draft-embed.md
示例参考：https://github.com/mattn/go-embed-example
tip 相关文档：https://tip.golang.org


### go-bindata
https://github.com/go-bindata/go-bindata

## 视频下载

https://github.com/iawia002/annie

抖音	    https://www.douyin.com	✓			
哔哩哔哩	https://www.bilibili.com	✓		✓	✓
半次元	    https://bcy.net		✓		
pixivision	https://www.pixivision.net		✓		
优酷	    https://www.youku.com	✓			✓
YouTube	    https://www.youtube.com	✓		✓	
爱奇艺	    https://www.iqiyi.com	✓			
芒果TV	    https://www.mgtv.com	✓			
糖豆广场舞	http://www.tangdou.com	✓		✓	
Tumblr	    https://www.tumblr.com	✓	✓		
Vimeo	    https://vimeo.com	✓			
Facebook	https://facebook.com	✓			
斗鱼视频	https://v.douyu.com	✓			
秒拍	    https://www.miaopai.com	✓			
微博	    https://weibo.com	✓			
Instagram	https://www.instagram.com	✓	✓		
Twitter	    https://twitter.com	✓			
腾讯视频	https://v.qq.com	✓			
网易云音乐	https://music.163.com	✓			
音悦台	    https://yinyuetai.com	✓			
极客时间	https://time.geekbang.org	✓			
Pornhub	    https://pornhub.com	✓			
XVIDEOS	    https://xvideos.com	✓			
聯合新聞網	https://udn.com	✓			
TikTok	    https://www.tiktok.com	✓			
好看视频    https://haokan.baidu.com	✓


## go包搜索
https://api.godoc.org/search?q=etcd
https://github.com/clearcodecn/gosearch

## go多版本
https://github.com/owenthereal/goup

## go测试
goc 是专为 Go 语言打造的一个综合覆盖率收集系统，尤其适合复杂的测试场景，比如系统测试时的代码覆盖率收集以及精准测试。
https://github.com/qiniu/goc

```
go tool cover -mode=count -var=CoverageVariableName xxxx.go
> 相信大家一定见过表示go覆盖率结果的coverprofile数据，类似下面: github.com/qiniu/goc/goc.go:21.13,23.2 1 1
其基本语义为 "文件:起始行.起始列,结束行.结束列 该基本块中的语句数量 该基本块被执行到的次数"
[聊聊 Go 代码覆盖率技术与最佳实践](https://xie.infoq.cn/article/ca1cc8ba293eddf793b3b0613)
```
## go Plugin
- go原生plugin
    - 实现机制基于动态链接so库
    - 跨语言支持较差且调用复杂，需解决不同语言的数据类型匹配问题

- github.com/hashicorp/go-plugin
    - 实现机制基于rpc调用，基于本地网络调用，调用性能高效
    - 插件可用多种语言实现，跨语言支持良好

## mock
- https://github.com/golang/mock
- https://github.com/brianvoe/gofakeit

## 机器学习
https://github.com/tensorflow/tensorflow/blob/master/tensorflow/go/README.md

[面部检测库](https://github.com/esimov/pigo)

## 流程图
https://github.com/blushft/go-diagrams
可以生成graphviz DOT file
go-diagrams实现了[diagrams](https://github.com/mingrammer/diagrams)的部分接口

## img/image
### Imagor
https://github.com/cshum/imagor
Imagor 是一个用 Go 编写的快速、支持 Docker 的图像处理服务器。
Imagor 使用最高效的图像处理库 libvips 之一（使用govips）。它通常比使用最快的 ImageMagick 和 GraphicsMagick 设置快4-8倍。
Imagor 是一个易于扩展的 Go 库，可以在任何 Unix 环境中安装和使用，并且可以使用 Docker 进行容器化。
Imagor 采用Thumbor URL 语法，涵盖了大多数 Web 图像处理用例。如果这些符合您的要求，Imagor 将是一种轻便、高性能的替代品。


## charts图表/plotting绘制/graphing绘图
### golang
https://github.com/vdobler/chart 695
https://github.com/Arafatk/glot 354
https://github.com/wcharczuk/go-chart 3.2k
https://github.com/gonum/plot 2k

### dotnet
https://github.com/ScottPlot/ScottPlot 1.1k
https://github.com/oxyplot/oxyplot 2.4k
https://github.com/Live-Charts/Live-Charts 5k
https://github.com/beto-rodriguez/LiveCharts2 771
https://github.com/microcharts-dotnet/Microcharts 1.6k
### 统计绘图
#### gnuplot
交互式绘图工具
http://www.gnuplot.info/

你可以在c#程序中编写数据文件，从c#调用gnuplot可执行文件，并在c#图片框中显示生成的图像。
#### Graph
http://www.padowan.dk/
#### Meta-chart.com
#### Infogr.am
https://infogram.com
#### Desmos
https://www.desmos.com/
#### Orange
https://orange.biolab.si/
#### GeoGebra
https://www.geogebra.org

#### AmCharts
https://www.amcharts.com


## http
### http中间件negroni
https://github.com/urfave/negroni
https://github.com/urfave/negroni#third-party-middleware

## web
- gin
https://github.com/gocopper/copper

## 扩展
### samber/lo 
https://github.com/samber/lo

### mapper
https://github.com/petersunbag/coven

这个也是很NB
https://github.com/jinzhu/copier

### 字符串format

https://github.com/sirkon/go-format 功能最强大
```
type t struct {
	A     string
	Field int
}
var s = t{
	A:     "str",
	Field: 12,
}
var d struct {
	F     t
	Entry float64
}
d.F = s
d.Entry = 0.5
res := format.Formatg("${F.A} ${F.Field} $Entry", d)
// res = "str 12 0.500000"
```

https://github.com/chonla/format

```
var params = map[string]interface{}{
    "sister": "Susan",
    "brother": "Louis",
}
format.Printf("%<brother> loves %<sister>.", params)
```

https://github.com/go-ffmt/ffmt/blob/master/format.go
```
Format("hello {name}", "ffmt") to "hello ffmt"
```

https://github.com/Wissance/stringFormatter
```
FormatComplex("Hello {user} what are you doing here {app} ?", map[string]interface{}{"user":"vpupkin", "app":"mn_console"})
```

https://github.com/delicb/gstring
```
// outpits "Bracket {, }, key value, key value, key value"
gstring.Printm("Bracket {{, }}, {key}, {key}, {key}", map[string]interaface{}{"key", "key value"})
```

https://github.com/mgenware/go-string-format
```
strf.Format("🐆{0} {1} {0}", "leopard", -3)
// returns "🐆leopard -3 leopard" 
```

https://github.com/taloric/strfmt
```
format_string := "Today is a {what} {desc}"
args := make(map[string]string)
args["what"] = "wonderful"
args["desc"] = "day"
res,err := strfmt.FormatMap(format_string, &args)
fmt.Println(res)
```

https://github.com/yangchenxing/go-string-mapformatter
```
fmt.Println(mapformatter.MustFormat("Hello %(name|s), you owe me %(money|.2f) dollar.",
    map[string]interface{}{
        "name": "anyone",
        "money": 10.3,
    }))
// Output: Hello anyone, you owe me 10.30 dollar.
```

https://github.com/christianhujer/tfmt/blob/master/main.go


https://golangdocs.com/string-formatting-in-golang
https://zetcode.com/golang/string-format/

### go-humanize
https://github.com/dustin/go-humanize
```
fmt.Printf("That file is %s.", humanize.Bytes(82854982)) // That file is 83 MB.
fmt.Printf("This was touched %s.", humanize.Time(someTimeInstance)) // This was touched 7 hours ago.
```
### dateparse
支持多种格式
https://github.com/araddon/dateparse

### 驼峰、下划线等转换
https://github.com/WnP/go-sfmt


## 其它
### 搜索引擎
https://github.com/prabhatsharma/zinc
### 一款用 SQL 方式查询 Git 仓库的开源项目进入 GitHub 趋势榜
https://github.com/augmentable-dev/gitqlite

### Webmail 服务器（SMTP、POP3、email）
https://github.com/inbucket/inbucket

### SSH

[massh](https://github.com/DiscoRiver/massh)：通过 SSH 方式运行 Linux 分布式 Shell 命令。

[sh](https://github.com/mvdan/sh): 一个支持 Bash 的 Shell 解析器、格式化器。