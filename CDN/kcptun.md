


## kcptun
https://github.com/xtaci/kcptun


1，增加文件打开数量。
```
ulimit -n 65535
```
2，调整udp包的相关参数。
```

net.core.rmem_max=26214400 // BDP - bandwidth delay product
net.core.rmem_default=26214400
net.core.wmem_max=26214400
net.core.wmem_default=26214400
net.core.netdev_max_backlog=2048 // proportional to -rcvwnd
```
3，增大buffer大小，默认的buffer大小为4MB。对于速度较慢的处理器，增加缓冲区对于正确接收数据包至关重要。
```

-sockbuf 16777217
```

4，kcptun启动的一个示例：
```

KCP Client: ./client_darwin_amd64 -r "KCP_SERVER_IP:4000" -l ":8388" -mode fast3 -nocomp -autoexpire 900 -sockbuf 16777217 -dscp 46
KCP Server: ./server_linux_amd64 -t "TARGET_IP:8388" -l ":4000" -mode fast3 -nocomp -sockbuf 16777217 -dscp 46
```
一、如果拥有高速网络连接，如何提高带宽使用？
同时增加kcptun客户端rcvnd参数和kcptun服务端sndnd参数。这些值中的最小值决定了链路的最大传输速率，如wnd * mtu / rtt。然后，尝试下载一些东西，看看它是否符合您的要求。MTU可以通过-mtu进行调整。
二、如果使用kcptun加速游戏，如何减小延迟？
延迟通常表示数据包丢失。您可以通过更改-mode参数来减少延迟。这里延迟从小到大的模式：fast3>fast2>fast>normal>default。
三、如何减少头部阻塞？
由于流被多路复用到单个物理信道中，因此可能会发生头部阻塞。将smuxbuf增加到更大值（默认为4MB）可能会缓解这个问题，但它会使用更多的内存。对于版本>= v20190924，可以切换到smux版本2。这个版本可以现在流内存使用。通过-streambuf可以限制每个流的内存使用。
四、性能低的设备如何调优？例如嵌入式设备？
kcptun使用Reed-Solomon码来恢复丢失的数据包，而这需要大量的计算。低端ARM设备可能无法很好地与kcptun配合使用。为了获得最佳性能，建议使用AMD、Intel等多核服务器CPU。如果必须使用ARM路由器，最好禁用FEC并使用salsa20作为加密方法。

## 其它加速
https://github.com/wangyu-/UDPspeeder


绕过UDP防火墙
https://github.com/wangyu-/udp2raw
https://github.com/Chion82/kcptun-raw


## 参考
https://zhuanlan.zhihu.com/p/453349832