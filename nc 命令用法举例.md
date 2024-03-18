# **nc 命令用法举例**

nc 是 netcat 的简写，有着网络界的瑞士军刀美誉。因为它短小精悍、功能实用，被设计为一个简单、可靠的网络工具

## **什么是 nc**

nc 是 netcat 的简写，有着网络界的瑞士军刀美誉。因为它短小精悍、功能实用，被设计为一个简单、可靠的网络工具

## **nc 的作用**

（1）实现任意 TCP/UDP 端口的侦听，nc 可以作为 server 以 TCP 或 UDP 方式侦听指定端口

（2）端口的扫描，nc 可以作为 client 发起 TCP 或 UDP 连接

（3）机器之间传输文件

（4）机器之间网络测速

## **nc 的控制参数不少，常用的几个参数如下所列：**

1) -l

用于指定 nc 将处于侦听模式。指定该参数，则意味着 nc 被当作 server，侦听并接受连接，而非向其它地址发起连接。

2) -p <port>

暂未用到（老版本的 nc 可能需要在端口号前加 - p 参数，下面测试环境是 centos6.6，nc 版本是 nc-1.84，未用到 - p 参数）

3) -s

指定发送数据的源 IP 地址，适用于多网卡机

4) -u

指定 nc 使用 UDP 协议，默认为 TCP

5) -v

输出交互或出错信息，新手调试时尤为有用

6）-w

超时秒数，后面跟数字

7）-z

表示 zero，表示扫描时不发送任何数据

## **前期准备**

准备两台机器，用于测试 nc 命令的用法

主机 A：ip 地址 10.0.1.161

主机 B：ip 地址 10.0.1.162

两台机器先安装 nc 和 nmap 的包

`yum install nc -y`

`yum install nmap -y`

如果提示如下 - bash：nc：command not found 表示没安装 nc 的包



![img](https://ask.qcloudimg.com/http-save/5426480/ntx41th0mn.png)



## **nc 用法 1，网络连通性测试和端口扫描**

**nc 可以作为 server 端启动一个 tcp 的监听（注意，此处重点是起 tcp，下面还会讲 udp）**

先关闭 A 的防火墙，或者放行下面端口，然后测试 B 机器是否可以访问 A 机器启动的端口

在 A 机器上启动一个端口监听，比如 9999 端口（注意：下面的 - l 是小写的 L，不是数字 1）

默认情况下下面监听的是一个 tcp 的端口

nc -l 9999



![img](https://ask.qcloudimg.com/http-save/5426480/45fhl6iwf2.png)



**客户端测试，测试方法 1**

在 B 机器上 telnet A 机器此端口，如下显示表示 B 机器可以访问 A 机器此端口



![img](https://ask.qcloudimg.com/http-save/5426480/zaidtvv89b.png)



**客户端测试，测试方法 2**

B 机器上也可以使用 nmap 扫描 A 机器的此端口

nmap 10.0.1.161 -p9999



![img](https://ask.qcloudimg.com/http-save/5426480/rxyuax7kfr.png)



**客户端测试，测试方法 3**

使用 nc 命令作为客户端工具进行端口探测

nc -vz -w 2 10.0.1.161 9999

（-v 可视化，-z 扫描时不发送数据，-w 超时几秒，后面跟数字）



![img](https://ask.qcloudimg.com/http-save/5426480/yxnb0w0d80.png)



上面命令也可以写成

nc -vzw 2 10.0.1.161 9999



![img](https://ask.qcloudimg.com/http-save/5426480/3hlj4shkaw.png)



**客户端测试，测试方法 4（和方法 3 相似，但用处更大）**

nc 可以可以扫描连续端口，这个作用非常重要。常常可以用来扫描[服务器](https://cloud.tencent.com/act/pro/promotion-cvm?from_column=20065&from=20065)端口，然后给[服务器安全](https://cloud.tencent.com/product/cwp?from_column=20065&from=20065)加固

在 A 机器上监听 2 个端口，一个 9999，一个 9998，使用 & 符号丢入后台



![img](https://ask.qcloudimg.com/http-save/5426480/9myyb50if3.png)



在客户端 B 机器上扫描连续的两个端口，如下



![img](https://ask.qcloudimg.com/http-save/5426480/54g9nars30.png)



**nc 作为 server 端启动一个 udp 的监听（注意，此处重点是起 udp，上面主要讲了 tcp）**

启动一个 udp 的端口监听

nc -ul 9998



![img](https://ask.qcloudimg.com/http-save/5426480/vidx66kdfl.png)



复制当前窗口输入 netstat -antup |grep 9998 可以看到是启动了 udp 的监听



![img](https://ask.qcloudimg.com/http-save/5426480/re8cherh0a.png)



**客户端测试，测试方法 1**

nc -vuz 10.0.1.161 9998

由于 udp 的端口无法在客户端使用 telnet 去测试，我们可以使用 nc 命令去扫描（前面提到 nc 还可以用来扫描端口）

（telnet 是运行于 tcp 协议的）

（u 表示 udp 端口，v 表示可视化输出，z 表示扫描时不发送数据）



![img](https://ask.qcloudimg.com/http-save/5426480/7x4utunfoq.png)



上面在 B 机器扫描此端口的时候，看到 A 机器下面出现一串 XXXXX 字符串



![img](https://ask.qcloudimg.com/http-save/5426480/5613wpqos7.png)



**客户端测试，测试方法 2**

nmap -sU 10.0.1.161 -p 9998 -Pn

（它暂无法测试 nc 启动的 udp 端口，每次探测 nc 作为 server 端启动的 udp 端口时，会导致对方退出侦听，有这个 bug，对于一些程序启动的 udp 端口在使用 nc 扫描时不会有此 bug）

下面，A 机器启动一个 udp 的端口监听，端口为 9998

在复制的窗口上可以确认已经在监听了



![img](https://ask.qcloudimg.com/http-save/5426480/i1azsh1yk1.png)



B 机器使用 nmap 命令去扫描此 udp 端口，在扫描过程中，导致 A 机器的 nc 退出监听。所以显示端口关闭了（我推测是扫描时发数据导致的）

nmap -sU 10.0.1.161 -p 9998 -Pn

-sU ：表示 udp 端口的扫描

-Pn ：如果服务器禁 PING 或者放在防火墙下面的，不加 - Pn 参数的它就会认为这个扫描的主机不存活就不会进行扫描了，如果不加 - Pn 就会像下面的结果一样，它也会进行提示你添加上 - Pn 参数尝试的



![img](https://ask.qcloudimg.com/http-save/5426480/4jlk348e3y.png)



注意：如果 A 机器开启了防火墙，扫描结果可能会是下面状态。（不能确定对方是否有监听 9998 端口）



![img](https://ask.qcloudimg.com/http-save/5426480/jj0bale9ei.png)



既然上面测试无法使用 nmap 扫描 nc 作为服务端启动的端口，我们可以使用 nmap 扫描其余的端口

（额，有点跑题了，讲 nmap 的用法了，没关系，主要为了说明 nmap 是也可以用来扫描 udp 端口的，只是扫描 nc 启动的端口会导致对方退出端口监听）

下面，A 机器上 rpcbind 服务，监听在 udp 的 111 端口



![img](https://ask.qcloudimg.com/http-save/5426480/qo9qfreift.png)



在 B 机器上使用 nmap 扫描此端口，是正常的检测到处于 open 状态



![img](https://ask.qcloudimg.com/http-save/5426480/pjluzpcv0d.png)



**客户端测试，测试方法 3**

**nc 扫描大量 udp 端口**

扫描过程比较慢，可能是 1 秒扫描一个端口，下面表示扫描 A 机器的 1 到 1000 端口（暂未发现可以在一行命令中扫描分散的几个端口的方法）

nc -vuz 10.0.1.161 1-1000



![img](https://ask.qcloudimg.com/http-save/5426480/0rnfpi5ehk.png)



## **nc 用法 2，使用 nc 传输文件和目录**

**方法 1，传输文件演示（先启动接收命令）**

使用 nc 传输文件还是比较方便的，因为不用 scp 和 rsync 那种输入密码的操作了

把 A 机器上的一个 rpm 文件发送到 B 机器上

需注意操作次序，receiver 先侦听端口，sender 向 receiver 所在机器的该端口发送数据。

**步骤 1，先在 B 机器上启动一个接收文件的监听，格式如下**

意思是把赖在 9995 端口接收到的数据都写到 file 文件里（这里文件名随意取）

nc -l port >file

nc -l 9995 >zabbix.rpm



![img](https://ask.qcloudimg.com/http-save/5426480/boenmr45t2.png)



**步骤 2，在 A 机器上往 B 机器的 9995 端口发送数据，把下面 rpm 包发送过去**

nc 10.0.1.162 9995 < zabbix-release-2.4-1.el6.noarch.rpm



![img](https://ask.qcloudimg.com/http-save/5426480/barxfug1gp.png)



B 机器接收完毕，它会自动退出监听，文件大小和 A 机器一样，md5 值也一样



![img](https://ask.qcloudimg.com/http-save/5426480/2yh0wdvf1w.png)



**方法 2，传输文件演示（先启动发送命令）**

**步骤 1，先在 B 机器上，启动发送文件命令**

下面命令表示通过本地的 9992 端口发送 test.mv 文件

nc -l 9992 <test.mv



![img](https://ask.qcloudimg.com/http-save/5426480/mk2vj0ku89.png)



**步骤 2，A 机器上连接 B 机器，取接收文件**

下面命令表示通过连接 B 机器的 9992 端口接收文件，并把文件存到本目录下，文件名为 test2.mv

nc 10.0.1.162 9992 >test2.mv



![img](https://ask.qcloudimg.com/http-save/5426480/f8wxrljkw8.png)



**方法 3，传输目录演示（方法发送文件类似）**

**步骤 1，B 机器先启动监听，如下**

A 机器给 B 机器发送多个文件

传输目录需要结合其它的命令，比如 tar

经过我的测试管道后面最后必须是 - ，不能是其余自定义的文件名

nc -l 9995 | tar xfvz -



![img](https://ask.qcloudimg.com/http-save/5426480/qfx9gsomnx.png)



**步骤 2，A 机器打包文件并连接 B 机器的端口**

管道前面表示把当前目录的所有文件打包为 - ，然后使用 nc 发送给 B 机器

tar cfz - * | nc 10.0.1.162 9995



![img](https://ask.qcloudimg.com/http-save/5426480/7cb7kors8p.png)



B 机器这边已经自动接收和解压



![img](https://ask.qcloudimg.com/http-save/5426480/193cjebdx5.png)



## **nc 用法 3，测试网速**

测试网速其实利用了传输文件的原理，就是把来自一台机器的 / dev/zero 发送给另一台机器的 / dev/null

就是把一台机器的无限个 0，传输给另一个机器的空设备上，然后新开一个窗口使用 dstat 命令监测网速

在这之前需要保证机器先安装 dstat 工具

yum install -y dstat

**方法 1，测试网速演示（先启动接收命令方式）**

步骤 1，A 机器先启动接收数据的命令，监听自己的 9991 端口，把来自这个端口的数据都输出给空设备（这样不写磁盘，测试网速更准确）

nc -l 9991 >/dev/null



![img](https://ask.qcloudimg.com/http-save/5426480/khhuquumhw.png)



步骤 2，B 机器发送数据，把无限个 0 发送给 A 机器的 9991 端口

nc 10.0.1.161 9991 </dev/zero



![img](https://ask.qcloudimg.com/http-save/5426480/9m3sraybj7.png)



在复制的窗口上使用 dstat 命令查看当前网速，dstat 命令比较直观，它可以查看当前 cpu，磁盘，网络，内存页和系统的一些当前状态指标。

我们只需要看下面我选中的这 2 列即可，recv 是 receive 的缩写，表示接收的意思，send 是发送数据，另外注意数字后面的单位 B，KB，MB



![img](https://ask.qcloudimg.com/http-save/5426480/iuig4f9dg6.png)



可以看到 A 机器接收数据，平均每秒 400MB 左右



![img](https://ask.qcloudimg.com/http-save/5426480/pyhldzpty3.jpeg)



B 机器新打开的窗口上执行 dstat，看到每秒发送 400MB 左右的数据



![img](https://ask.qcloudimg.com/http-save/5426480/zkdl7i21l4.jpeg)



**方法 2，测试网速演示（先启动发送命令方式）**

步骤 1，先启动发送的数据，谁连接这个端口时就会接收来自 zero 设备的数据（二进制的无限个 0）

nc -l 9990 </dev/zero



![img](https://ask.qcloudimg.com/http-save/5426480/ljvkk65vc9.png)



步骤 2，下面 B 机器连接 A 机器的 9990 端口，把接收的数据输出到空设备上

nc 10.0.1.161 9990 >/dev/null



![img](https://ask.qcloudimg.com/http-save/5426480/f1bpwthomq.png)



同样可以使用 dstat 观察数据发送时的网速



![img](https://ask.qcloudimg.com/http-save/5426480/dc84lcrf8.png)

