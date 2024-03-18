masscan 使用教程Masscan 教程和入门手册 - 简书

简介：

**开源、免费的端口扫描器，获取主机开放的端口和端口信息。**

**速度非常快，6 分钟可以扫描整个互联网，1 台机器每秒可传输 1000 万个数据包。**

**只做 SYN 扫描、不首先 ping 主机，没有 DNS 解析发生，扫描完全随机化**

**masscan 是为了尽可能快地扫描整个互联网而创建的，根据其作者 robert graham，这可以在不到 6 分钟内完成，每秒大约 1000 万个数据包。**

[下载链接](https://download.csdn.net/download/qq_46631866/12403651)

### 原理：



MASSCAN 不建立完整的 TCP 连接，收到 SYN/ACK 之后，发送 RST 结束连接。选项 --banners 除外。



![img](http://upload-images.jianshu.io/upload_images/9272355-8f3a51456832b54f.png)





![img](http://upload-images.jianshu.io/upload_images/9272355-f19824f1445079e9.png)





***单个端口扫描***

扫描 443 端口的 B 类子网



```
masscan 192.168.0.0/16 -p 443
```



![img](http://upload-images.jianshu.io/upload_images/9272355-640bedafcec6ef75.png)



***多端口扫描***
扫描 80 或 443 端口的 B 类子网



```
masscan 192.168.0.0/16 -p80,443
```



![img](http://upload-images.jianshu.io/upload_images/9272355-70956a2814fe6014.png)



***扫描一系列端口***
扫描 22 到 25 端口的 B 类子网



```
masscan 192.168.0.0/16 -p22-25
```



![img](http://upload-images.jianshu.io/upload_images/9272355-217a55662b777738.png)



***快速扫描***
默认情况下，Masscan 扫描速度为每秒 100 个数据包，这是相当慢的。为了增加这一点，只需提供该 - rate 选项并指定一个值。
扫描 100 个常见端口的 B 类子网，每秒 100,000 个数据包



```
masscan 192.168.0.0/16 --ports 0-100 --rate 10000
```



你可以扫描的速度取决于很多因素，包括您的操作系统（Linux 扫描扫描远远快于 Windows），系统的资源，最重要的是您的带宽。为了以高速扫描非常大的网络，您需要使用百万以上的速率（--rate 1000000）。



扫描结果保存



```
masscan 192.168.0.0/16 -p80 -oX test.xml（-oJ xx.json -oL xx.list)
```

## 扫描指定网段范围的指定端口 

1. masscan -p80,8080-8100 10.0.0.0/8
2. （扫描 10.x.x.x 子网，扫描端口 80 和 8000-8100 范围的端口段）

 也可以 –echo 将当前的配置输出到一个配置文件，利用 -c 来制定配置文件进行扫描

1. masscan -p80,8000-8100 10.0.0.0/8 --echo > xxx.conf
2. *masscan -c xxx.conf --rate 1000*

## 获取 Banner



```
masscan 10.0.0.0/8 -p80 --banners --source-ip x.x.x.x
```

扫描 10.x.x.x 网段 80 端口的开放信息，并且获取 banner 信息。–source-ip 是指定源 IP，这个 ip 必须指定独立有效的 IP 地址。

## 设置扫描时忽略一些网段 



```
masscan 0.0.0.0/0 -p0-65535 --excludefile exclude.txt
```

## 输出到指定文件中



```
masscan 0.0.0.0/0 -p0-65535 -oX scanRes.xml
```

## 设置扫描速度



```
masscan 0.0.0.0/0 -p0-65535 --max-rate 100000
```

扫描器使用的默认的速率是 100 包 / 秒，这条命令将以每秒 10 万包的速率进行扫描。

## *用加载配置文件的方式运行*

配置文件的内容如下所示：

rate = 100000
output-format = xxx
output-status = all
output-filename = xxx.xxx
ports = 0-65535
range = 0.0.0.0-255.255.255.255
excludefile = exclude.txt

扫描时，用 -c 加载配置文件。 

## 结果输出

1、XML 默认格式 使用 - oX <filename> 或者使用 –output-format xml 和 –output-filename <filename > 进行指定
2、binary masscan 内置格式
masscan --open --banners --readscan output.txt -oX 2.txt　打开显示模式，读取 output.txt 中的数据，并以 xml 的格式写到 2.txt 中 3、grepable nmap 格式 使用 -oG <filename> 或者 –output-format grepable 和 –output-filename <filename > 进行指定
4、json 使用 -oJ <filename> 或者 –output-format json 和 –output-filename <filename > 进行指定
5、list 简单的列表, 每行一个主机端口对。使用 - oL <filename> 或者 –output-format list 和 –output-filename <filename > 进行指定

## 命令行模式详细参数

<ip/range> IP 地址范围，有三种有效格式，1、单独的 IPv4 地址 2、类似 "10.0.0.1-10.0.0.233" 的范围地址 3、CIDR 地址 类似于 "0.0.0.0/0"，多个目标可以用都好隔开

-p <ports,--ports <ports>> 指定端口进行扫描

--banners 获取 banner 信息，支持少量的协议

--rate <packets-per-second> 指定发包的速率

-c <filename>, --conf <filename> 读取配置文件进行扫描

--echo 将当前的配置重定向到一个配置文件中

-e <ifname> , --adapter <ifname> 指定用来发包的网卡接口名称

--adapter-ip <ip-address> 指定发包的 IP 地址

--adapter-port <port> 指定发包的源端口

--adapter-mac <mac-address> 指定发包的源 MAC 地址

--router-mac <mac address> 指定网关的 MAC 地址

--exclude <ip/range> IP 地址范围黑名单，防止 masscan 扫描

--excludefile <filename> 指定 IP 地址范围黑名单文件

--includefile，-iL <filename> 读取一个范围列表进行扫描

--ping 扫描应该包含 ICMP 回应请求

--append-output 以附加的形式输出到文件

--iflist 列出可用的网络接口，然后退出

--retries 发送重试的次数，以 1 秒为间隔

--nmap 打印与 nmap 兼容的相关信息

--http-user-agent <user-agent> 设置 user-agent 字段的值

--show [open,close] 告诉要显示的端口状态，默认是显示开放端口

--noshow [open,close] 禁用端口状态显示

--pcap <filename> 将接收到的数据包以 libpcap 格式存储

--regress 运行回归测试，测试扫描器是否正常运行

--ttl <num> 指定传出数据包的 TTL 值，默认为 255

--wait <seconds> 指定发送完包之后的等待时间，默认为 10 秒

--offline 没有实际的发包，主要用来测试开销

-sL 不执行扫描，主要是生成一个随机地址列表

--readscan <binary-files> 读取从 - oB 生成的二进制文件，可以转化为 XML 或者 JSON 格式.

--connection-timeout <secs> 抓取 banners 时指定保持 TCP 连接的最大秒数，默认是 30 秒。