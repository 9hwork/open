[TOC]
## CPU
CPU统计工具如topas、sar、vmstat、lparstat、iostat

如果要查看每个逻辑cpu的使用率，只需要运行top命令，按下数字键1即可

top执行一次:`top -n 1`

## memory
[内存测试工具](https://www.cnblogs.com/sunshine-blog/p/11903842.html)

> [cpu测试脚本](../linux/shell/cpuload.sh) ;  [内存测试脚本](../linux/shell/cpuload.sh)

## I/O

狭义讲，IO分两类：

1. 网络 Network
2. 磁盘 Disk or Storage

衡量磁盘性能常见的指标有: 使用率、饱和度、IOPS、吞吐量以及响应时间，具体说明如下：
- **使用率**，是指磁盘处理 I/O 的时间百分比。过高的使用率（比如超过 80%），通常意味着磁盘 I/O 存在性能瓶颈。
- **饱和度**，是指磁盘处理 I/O 的繁忙程度。过高的饱和度，意味着磁盘存在严重的性能瓶颈。当饱和度为 100% 时，磁盘无法接受新的 I/O 请求。
- **IOPS**（Input/Output Per Second），是指每秒的 I/O 请求数。
- **吞吐量**，是指每秒的 I/O 请求大小，即每秒磁盘 I/O 的流量，磁盘写入加上读出数据的大小。单位为bps。
- **响应时间**，是指 I/O 请求从发出到收到响应的间隔时间。

### 磁盘类型

- 硬盘接口类型：有`IDE=ATA<SATA,  SCSI<SAS  NVME(适用于SSd)  光纤通道硬盘接口`
- 硬盘材质类型：分为机械和固态:HDD SSD
- 硬盘的使用方式：单块或者raid:raid用raid控制器代替磁盘控制器
- 硬盘机柜网络：有FC网络的和普通网线的网络
- 硬盘+服务器的存储架构：DAS=server+raid SAN=server--raid存储器   NAS=server+rais存储器+文件系统

硬盘接口通常分为五种类型：SATA接口硬盘、IDE接口硬盘、SCSI接口硬盘、光纤通道硬盘、SAS接口硬盘。

SSD采用闪存颗粒来存储，HDD采用磁性碟片来存储，混合硬盘(HHD: Hybrid Hard Disk)是把磁性硬盘和闪存集成到一起的一种硬盘。


/dev/sda是第一个检测到的IDE / SATA / SCSI类型的磁盘。在这种情况下，由管理程序进行仿真（完全虚拟化）。
/dev/vda是第一个检测到的半虚拟化磁盘驱动程序。

/dev/vda 和 /dev/vdb 都是 virtio-block 类型的设备，而 /dev/sda 是 sd 即 SCSI 类型的设备。 

virtio-blk 设备的名称以 ‘vd’ 开头。从  ‘vda’ 开始递增，数目在 26 个以内时，增长至 ‘vdz’；如果超过 26，则从 ’vdaa‘ 一直增长至 ’vdzz‘；最高可以增长到 ’vdzzz‘。

常见的命名：

- fd：软驱
- hd：IDE 磁盘
- sd：SCSI 磁盘
- tty：terminals
- vd：virtio 磁盘

硬盘三大种类（SSD；HHD；HDD）
- 固态硬盘（Solid State Drive）: 用固态电子存储芯片阵列而制成的硬盘，由控制单元和存储单元（FLASH芯片、DRAM芯片）组成。
- 混合硬盘（hybrid harddrive，HHD）: 是既包含传统硬盘又有闪存（flashmemory）模块的大容量存储设备。
- 传统硬盘（HDD，Hard Disk Drive的缩写）: 传统硬盘，即机械硬盘；

### IOPS

IOPS (Input/Output Operations Per Second)，即每秒进行读写（I/O）操作的次数。

IOPS (Input/Output Per Second)即每秒的输入输出量(或读写次数)，是衡量磁盘性能的主要指标之一。IOPS是指单位时间内系统能处理的I/O请求数量，一般以每秒处理的I/O请求数量为单位，I/O请求通常为读或写数据操作请求。

随机读写频繁的应用，如小文件存储(图片)、OLTP数据库、邮件服务器，关注随机读写性能，IOPS是关键衡量指标。 顺序读写频繁的应用，传输大量连续数据，如电视台的视频编辑，视频点播VOD(Video On Demand)，关注连续读写性能。数据吞吐量(Throughput)是关键衡量指标。

读取10000个1KB文件，用时10秒 Throught(吞吐量)=1MB/s ，IOPS=1000 追求IOPS；
读取1个10MB文件，用时0.2秒 Throught(吞吐量)=50MB/s, IOPS=5 追求吞吐量(Throughput)；
> 小文件追求IOPS, 大文件追求吞吐量(Throughput)

### Throughput (吞吐) or Bandwidth （带宽）
磁盘的吞吐量，也就是每秒磁盘 I/O 的流量，即磁盘写入加上读出的数据的大小。(指每秒的 I/O 请求大小)

每秒 I/O 吞吐量＝ IOPS* 平均 I/O SIZE。

**响应时间**，是指 I/O 请求从发出到收到响应的间隔时间。



### 相关工具

sar -d, iostat, topas, nmon, iotop

#### 硬盘IO读写速度测试
**硬盘IO写速度测试**
1. 测试磁盘速度:`hdparm -Tt /dev/sda3`
2. dd
测试逻辑速度【结果较快】
表示 每次写入8k的数据，执行300000次
`time dd if=/dev/zero of=test.dbf bs=8k count=300000`

测试真实的IO速度，需要在后面加上参数oflag=direct 【这个过程较慢】
`time dd if=/dev/zero of=test.dbf bs=8k count=300000 oflag=direct`
上面操作会在当前路径留下一个test文件，记得删除啊
```
[root@centos-73-iso-100g-test data_vdb1]# du -sh * | tail -n1
2.3G    test.dbf
[root@centos-73-iso-100g-test data_vdb1]# rm -rf test.dbf 
```

**硬盘IO读速度测试**
测试逻辑速度【结果较快】
表示 每次读取8k的数据，执行300000次
`dd if=test.dbf bs=8k count=300000 of=/dev/null `

真实测试
`dd if=test.dbf bs=8k count=300000 of=/root/test2.dbf oflag=direct`
或者 创建一个3G的文件
`dd if=/dev/zero of=test.txt bs=1M count=3000`

一般它的常用参数有：

- if=初始路径
- of=目的路径
- bs=n，block size，每次读取 n bytes 写入，可与 count 联用；
- ibs=n，一次读入 bytes 个字节 (default is 512)；
- obs=n，一次性写 n bytes 个字节 (default is 512)；
- bs= 可以同时设置上边两个参数；
- cbs=n，一次转换 n 个 bytes，即转换缓冲区大小。；
- count=n， bs 操作的次数，仅拷贝 n 个块，如 dvd: - bs=1M count=4430；
- skip=n，指 if 后面的原文件跳过 n bytes 再开始读取；
- seek=n，指 of 后面的目标文件跳过 n bytes 再开始写入；

查看目录下所有文件的大小并按照大小排序 : `du -sh * | sort -rh`
统计当前目录的大小:`du -sh`
查看当前目录下所有一级子目录文件夹大小 并排序: `sudo du -h --max-depth=1 |sort`
以人性化的方式显示文件大小:`du -h Debian.iso`
查看当前目录下一级子文件和子目录占用的磁盘容量:`du -lh --max-depth=1`

#### iostat
iostat，对系统的磁盘操作活动进行监视。它的特点是汇报磁盘活动统计情况，同时也会汇报出CPU使用情况。**iostat也有一个弱点，就是它不能对某个进程进行深入分析，仅对系统的整体情况进行分析。**

`yum -y install sysstat`

- 展示所有的磁盘I/O指标:`iostat -d -x 1` ; 间隔1秒，总共显示3次: `iostat -d -x 1 3`
• %util ，就是我们前面提到的磁盘 I/O 使用率；
• r/s+ w/s ，就是 IOPS；
• rkB/s+wkB/s ，就是吞吐量；
• r_await+w_await ，就是响应时间。

- 查看cpu状态:`iostat -c 1 1`
- 每隔2秒显示一次sda, sdb两个设备的扩展统计信息,共输出3次 : `iostat -x sda sdb 2 3`
- 每隔2秒显示一次sda及上面所有分区的统计信息,共输出3次 : `iostat -p sda 2 3`


命令参数说明：

-c： 显示CPU使用情况
-d： 显示磁盘使用情况
-N： 显示磁盘阵列(LVM) 信息
-n： 显示NFS 使用情况
-k： 以 KB 为单位显示
-m： 以 M 为单位显示
-t： 报告每秒向终端读取和写入的字符数和CPU的信息
-V： 显示版本信息
-x： 显示IO相关的详细信息
-p [磁盘] ： 显示磁盘和分区的情况

> 备注：
如果%iowait的值过高，表示硬盘存在I/O瓶颈，%idle值高，表示CPU较空闲。
如果%idle值高但系统响应慢时，有可能是CPU等待分配内存，此时应加大内存容量。
如果%idle值持续低于10，那么系统的CPU处理能力相对较低，表明系统中最需要解决的资源是CPU。

CPU属性值说明：
- %user： CPU处在用户模式下的时间百分比
- %nice： CPU处在带NICE值的用户模式下的时间百分比
- %system： CPU处在系统模式下的时间百分比
- %iowait： CPU等待输入输出完成时间的百分比
- %steal： 管理程序维护另一个虚拟处理器时，虚拟CPU的无意识等待时间百分比
- %idle： CPU空闲时间百分比

磁盘属性值说明：

- device: 磁盘名称
- tps: 每秒钟发送到的I/O请求数
- Blk_read/s: 每秒读取的block数
- Blk_wrtn/s: 每秒写入的block数
- Blk_read: 读入的block总数
- Blk_wrtn: 写入的block总数

磁盘IO相关的详细说明：

- rrqm/s: 每秒进行 merge 的读操作数目。即 rmerge/s
- wrqm/s: 每秒进行 merge 的写操作数目。即 wmerge/s
- r/s: 每秒完成的读 I/O 设备次数。即 rio/s
- w/s: 每秒完成的写 I/O 设备次数。即 wio/s
- rkB/s: 每秒读K字节数。是 rsect/s 的一半，因为每扇区大小为512字节。
- wkB/s: 每秒写K字节数。是 wsect/s 的一半
- avgrq-sz: 平均每次设备I/O操作的数据大小 (扇区)
- avgqu-sz: 平均I/O队列长度
- rsec/s: 每秒读扇区数。即 rsect/s
- wsec/s: 每秒写扇区数。即 wsect/s
- r_await: 每个读操作平均所需的时间，不仅包括硬盘设备读操作的时间，还包括了在kernel队列中等待的时间
- w_await: 每个写操作平均所需的时间，不仅包括硬盘设备写操作的时间，还包括了在kernel队列中等待的时间
- await: 平均每次设备I/O操作的等待时间 (毫秒)
- svctm: 平均每次设备I/O操作的服务时间 (毫秒)
- %util: 一秒中有百分之多少的时间用于 I/O 操作，即被io消耗的cpu百分比

> 备注：
如果 %util 接近 100%，说明产生的I/O请求太多，I/O系统已经满负荷，该磁盘可能存在瓶颈。
如果 svctm 比较接近 await，说明 I/O 几乎没有等待时间；
如果 await 远大于 svctm，说明I/O 队列太长，io响应太慢，则需要进行必要优化。
如果avgqu-sz比较大，也表示有当量io在等待。

#### pidstat
pidstat，用于监控全部或指定进程的cpu、内存、线程、设备IO等系统资源的占用情况。pidstat首次运行时显示自系统启动开始的各项统计信息，之后运行pidstat将显示自上次运行该命令以后的统计信息。用户可以通过指定统计的次数和时间来获得所需的统计信息。

命令参数说明：
- -u：默认的参数，显示各个进程的cpu使用统计
- -r：显示各个进程的内存使用统计
- -d：显示各个进程的IO使用情况
- -p：指定进程号
- -w：显示每个进程的上下文切换情况
- -t：显示选择任务的线程的统计信息外的额外信息
- -T TASK CHILD | ALL ：TASK表示报告独立的task，CHILD关键字表示报告进程下所有线程统计信息。ALL表示报告独立的task和task下面的所有线程。注意：task和子线程的全局的统计信息和pidstat选项无关。这些统计信息不会对应到当前的统计间隔，这些统计信息只有在子线程kill或者完成的时候才会被收集
- -V：显示版本号
- -h：在一行上显示了所有活动，这样其他程序可以容易解析
- -I：在SMP环境，表示任务的CPU使用率/内核数量
- -l：显示命令名和所有参数

1. 观测进程的I/O性能指标:`pidstat -d`
属性值说明：
- PID：进程ID
- kB_rd/s：每秒从磁盘读取的KB
- kB_wr/s：每秒写入磁盘KB
- kB_ccwr/s：任务取消的写入磁盘的KB。当任务截断脏的pagecache的时候会发生。
- COMMAND：task的命令名


2. 查看所有进程的CPU使用情况: `pidstat -u -p ALL`

属性值说明：
- PID：进程ID
- %usr：进程在用户空间占用cpu的百分比
- %system：进程在内核空间占用cpu的百分比
- %guest：进程在虚拟机占用cpu的百分比
- %CPU：进程占用cpu的百分比
- CPU：处理进程的cpu编号
- Command：当前进程对应的命令

3. 查看指定进程的内存使用情况: `pidstat -r -p 29468 1 4`
指定PID为29468的进程内存使用情况，每秒展示一次，展示四次！也可以直接pidstat -r，是全部进程的内存使用情况！

属性值说明：
- PID：进程标识符
- Minflt/s：任务每秒发生的次要错误，不需要从磁盘中加载页
- Majflt/s：任务每秒发生的主要错误，需要从磁盘中加载页
- VSZ：虚拟地址大小，虚拟内存的使用KB
- RSS：常驻集合大小，非交换区内存的使用KB
- Command：task命令名

4. 进程的上下文切换情况: `pidstat -w`
属性值说明：

- PID：进程ID
- cswch/s：每秒主动任务上下文切换数量
- nvcswch/s：每秒被动任务上下文切换数量
- Command：命令名

5. 显示特定进程的线程统计情况: `pidstat -p 12920 -t`
属性值说明：

- TGID：主线程的表示
- TID：线程id
- %usr：进程在用户空间占用cpu的百分比
- %system：进程在内核空间占用cpu的百分比
- %guest：进程在虚拟机占用cpu的百分比
- %CPU：进程占用cpu的百分比
- CPU：处理进程的cpu编号
- Command：当前进程对应的命令

#### iotop
iotop是一个用来监视磁盘I/O使用状况的 top 类工具，可监测到哪一个程序使用的磁盘IO的信息。

`yum -y install iotop`

命令参数说明：
- -o, --only只显示正在产生I/O的进程或线程。除了传参，可以在运行过程中按o生效。
- -b, --batch非交互模式，一般用来记录日志。
- -n NUM, --iter=NUM设置监测的次数，默认无限。在非交互模式下很有用。
- -d SEC, --delay=SEC设置每次监测的间隔，默认1秒，接受非整形数据例如1.1。
- -p PID, --pid=PID指定监测的进程/线程。
- -u USER, --user=USER指定监测某个用户产生的I/O。
- -P, --processes仅显示进程，默认iotop显示所有线程。
- -a, --accumulated显示累积的I/O，而不是带宽。
- -k, --kilobytes使用kB单位，而不是对人友好的单位。在非交互模式下，脚本编程有用。
- -t, --time 加上时间戳，非交互非模式。
- -q, --quiet 禁止头几行，非交互模式。有三种指定方式，其中，-q表示只在第一次监测时显示列名，-qq表示永远不显示列名，-qqq表示永远不显示I/O汇总。

交互按键：
和top命令类似，iotop也支持以下几个交互按键。
- left和right方向键：改变排序。
- r：反向排序。
- o：切换至选项--only。
- p：切换至--processes选项。
- a：切换至--accumulated选项。
- q：退出。
- i：改变线程的优先级。

1. 只显示正在产生I/O的进程或线程（交互式）
除了传参，可以在运行过程中按o生效。
`iotop  -o`

2. 按时间间隔刷新（交互式）
每间隔2秒，输出5次。
`iotop  -d 2 -n 5`

3. 按时间间隔刷新，输出到屏幕（非交互式）
每间隔2秒，输出5次。也可输出到日志文本，用于监控某时间段的io信息.
`iotop -botq -n 5 -d 2 `

4. 输出PID为8382的进程的磁盘IO信息（非交互式）
`iotop -botq -p 8382`

#### pt-ioprofile
https://github.com/percona/percona-toolkit/

pt-ioprofile工具是Percona-toolkit工具包中用来分析MySQL各个文件IO活动的小工具，pt-ioprofile工具需要用root用户执行且依赖于lsof和strace命令,该工具的基本逻辑如下
    1. 使用lsof和strace采集数据
    2. 汇聚采集的结果，汇聚规则可以是sum或avg

```
yum install lsof strace -y
wget https://www.percona.com/downloads/percona-toolkit/3.1.0/binary/redhat/7/x86_64/percona-toolkit-3.1.0-2.el7.x86_64.rpm
yum install -y percona-toolkit-3.1.0-2.el7.x86_64.rpm
pt-ioprofile --version
```
因strace在CentOS6和CentOS7上输出的头信息格式变化，导致该工具在CentOS7下目前存在BUG需要修改脚本，详细BUG信息可查看以下链接
https://jira.percona.com/browse/PT-1631
```
## 修改脚本中574行对strace的匹配语法
shell> vim /usr/bin/pt-ioprofile +573
## 修改前
573    /^COMMAND/ { mode = "lsof";   }
574    /^Process/ { mode = "strace"; }  
 
## 修改后
573    /^COMMAND/ { mode = "lsof";   }
574    /^(strace: )?Process/ { mode = "strace"; } 
```

在3.5.1版本中已经修复
```
yum install lsof strace -y
wget https://www.percona.com/downloads/percona-toolkit/3.5.4/binary/redhat/7/x86_64/percona-toolkit-3.5.4-2.el7.x86_64.rpm
yum install -y percona-toolkit-3.5.4-2.el7.x86_64.rpm
pt-ioprofile --version
```


`pt-ioprofile --profile-pid=1236 --cell=sizes`
 
`pt-ioprofile --profile-pid=$(pidof mysqld) --cell=count --run-time=5`

> pt-ioprofile会冻结服务器，并可能使进程崩溃，或在分离后使其性能下降，或使其处于睡眠状态,pt-ioprofile是一种侵入性工具，不应在生产服务器上使用pt-ioprofile。

### 网络IO

`ss`命令查看socket连接数量
ss是Socket Statistics的缩写。顾名思义，ss命令可以用来获取socket统计信息，它可以显示和netstat类似的内容。ss的优势在于它能够显示更多更详细的有关TCP和连接状态的信息，而且比netstat更快速更高效。

`lsof`命令可以查看所有已经打开了的文件，比如: 普通文件，目录，特殊的块文件，管道，socket套接字，设备，Unix域套接字等等(lsof是List Open File(List Open File), 在Linux中，一切皆文件)

#### 服务器网络流量监控工具：Ntopng
http://www.ntop.org/

```
sudo apt-get install ntopng

sudo vim /etc/ntopng.conf
```

/etc/ntopng.conf
```
# DO NOT REMOVE the following option, required for daemonization.
-e=

# * Interfaces to sniff on: one interface per line, prefix with -i=
# If none is specified, ntopng will try to auto-detect the best interface.

-i=eth0

# * Port on which ntopng will listen for the web-UI.
-w=3000
```

`sudo systemctl restart ntopng`

防火墙中开启相应的端口: `sudo ufw allow 3000`

#### 服务器网络流量监控工具：Munin
http://munin-monitoring.org/
Munin可以说是一个综合性的服务器性能监控平台，除了可以得到网络流量等信息，还可以看到硬盘容量、IO读写、CPU使用、内存占用等各类信息，比较适合用于服务器的全方位监控。

Debian或者Ubuntu系统：`apt-get install munin munin-node`
如果是Redhat或者CentOS系统：`yum install munin munin-node`。


#### mtr网络链路路由测试工具
mtr（My traceroute）集合ping、tracerouted的特性，功能更强大。mtr默认发送ICMP数据包进行链路探测，用户还可以通过-u参数来指定使用UDP数据包用于探测。相比traceroute只会做一次链路跟踪测试，mtr会对链路上的相关节点做持续探测并给出相应的统计信息。mtr能避免节点波动对测试结果的影响，所以其测试结果更正确。

WinMTR下载地址：https://sourceforge.net/projects/winmtr/files/WinMTR-v092.zip/download

指标:
Interval（sec）：每次探测的间隔（过期）时间，默认为1秒
ping size(bytes)：ping探测所使用的数据包大小，默认为64字节
Max hosts in LRU list：LRU列表支持的最大主机数，默认值为128
Resolve names：通过反查IP地址，以域名显示相关节点。

Linux下使用mtr命令
命令也很简单：mtr 服务器ip或者域名
mtr命令可选参数：
-r或—report：以报告模式显示输出；
-p或—split：将每次追踪的结果分别列出来，而非如 —report统计整个结果；
-s或—psize：指定ping数据包的大小；
-n或—no-dns：不对IP地址做域名反解析；
-a或—address：设置发送数据包的IP地址，用于主机有多个IP时；
-4：只使用IPv4协议；
-6：只使用IPv6协议；

mtr在运行过程中，可以通过输入相应字母来快速切换模式：
？或h：显示帮助菜单；
d：切换显示模式；
n：切换启用或禁用DNS域名解析；
u：切换使用ICMP或UDP数据包进行探测；

mtr测试结果每列数值的说明如下：
Host：节点IP地址和域名（按n键可以切换显示）；
Loss%：节点丢包率；
Snt：每秒发送数据包数，默认值是10，可以通过参数-c指定；
Last：最近一次的探测延迟值；
Avg：探测延迟的平均值；
Best：探测延迟的最小值；
Wrst：探测延迟到最大值；
StDev：标准偏差值，越大说明相应节点越不稳定。



### http请求耗时
[httpstat](https://github.com/davecheney/httpstat)
`httpstat https://example.com/ `
可以看到 `DNS Lookup`, `TCP Connection`, `TLS Handshake`(http没有, 其它的顺序一样), `Server Processing`, `Content Transfer`

原理就是利用httptrace.ClientTrace来做各个阶段的监控