## nc的基本用法

​    nc(netcat) 被誉为网络安全界的‘瑞士军刀’，可以用于完成几乎涉及TCP、UDP或者Unix域套接字的任何事。它可以打开TCP连接，发送UDP报文，在任意的TCP和UDP端口监听，进行端口扫描，支持ipv6。不象telnet，nc能够更好地支持脚本，能够将错误消息分离到标准错误，而不是标准输出。nc有四种典型应用：

#### 一、C/S模型

用nc能够非常简单地建立一个基本的C/S模型。
打开控制台，在某个端口上启动nc监听，等待连接。例如，nc在端口1234上启动监听，等待连接：
$ nc -l 1234 

在另外一个控制台上（或另外一台机器上），连接该机器上监听的端口 
$ nc 127.0.0.1 1234

现在，端口之间的连接就建立了。连接建立之后，nc并不区分谁是server，谁是client。在任何一端的输入都会发送到另一端。通过发送EOF（^D’）中断连接。

Linux中的nc没有-c或-e选项（可能是安全因素），但是，你仍然能够通过重定向文件描述符在建立连接之后执行命令。注意，打开一个端口，让任何人连接并在你的机器上执行任意命令是危险的。如果你需要这样做，下面有个例子：

在server端:
$ rm -f /tmp/f; mkfifo /tmp/f
$ cat /tmp/f | /bin/sh -i 2>&1 | nc -l 127.0.0.1 1234 > /tmp/f

在client端:
$ nc host.example.com 1234
$ (shell prompt from host.example.com)

通过创建一个FIFO文件/tmp/f，让nc在server端127.0.0.1的端口1234上监听，当一个client成功连接之后，/bin/sh在服务端执行，但执行反馈传送给client端。
当连接中断后，nc也退出。如果要保持连接，使用-k选项，但是如果命令退出，这个选项也不会重启它或保持nc运行。如果不再需要，不要忘记删除该文件描述符。
$ rm -f /tmp/f

#### 二、数据传输

上面的例子可以扩展来建立一个基本的数据传输模型。在连接的一端输入的任何信息将输出到另一端，输入和输出能够很容易被捕捉来模拟文件传输。
nc在一个端口上启动监听，并将输出重定向到一个文件中：
$ nc -l 1234 > filename.out

在另一台机器上，连接nc运行的机器和监听端口，将要传输的文件作为输入：
$ nc host.example.com 1234 < filename.in

当文件传输完毕后，连接自动关闭。

#### 三、和服务器通讯

在解决故障和调试中，有时候手动和服务器通信很有用，而不是通过一个用户界面。比如，当需要验证一个服务器对于client发送的命令有什么响应时。例如，获得一个Web站点的主页：
$ printf "GET / HTTP/1.0\r\n\r\n" | nc host.example.com 80

注意，上述命令也会显示Web服务器发送的页面头部。可以根据需要用sed等工具过滤返回的信息。 

可以建立更复杂的例子，当用户知道服务器端要求的请求格式。例如，一封email可以提交给SMTP邮件发送服务器：

$ nc [-C] localhost 25 << EOF
HELO host.example.com
MAIL FROM:<[user@host.example.com](mailto:user@host.example.com)>
RCPT TO:<[user2@host.example.com](mailto:user2@host.example.com)>
DATA
Body of email.
.
QUIT
EOF

#### 四、端口扫描

有时候需要知道目标机器上哪些端口开放和运行了那些服务。nc可以利用-z标志来获取开放的端口。通常和-v标志一起用来向标准错误输出详细信息。例如：
$ nc -zv host.example.com 20-30
Connection to host.example.com 22 port [tcp/ssh] succeeded!
Connection to host.example.com 25 port [tcp/smtp] succeeded!

指定搜索的端口范围为20-30，并且按升序进行扫描。你也可以指定一个端口列表来扫描， 端口按给定的顺序进行扫描。例如:
$ nc -zv host.example.com 80 20 22
nc: connect to host.example.com 80 (tcp) failed: Connection refused
nc: connect to host.example.com 20 (tcp) failed: Connection refused
Connection to host.example.com port [tcp/ssh] succeeded!

有时候需要了解哪些服务软件以及什么版本正在运行。这些信息通常包含在问候标语(greeting banner)中。为了取得这些信息，首先需要建立一个连接，然后当问候标语获得之后再断开连接。这能够通过用-w标志指定一个短暂的超时时间来实现，或者也许能通过发送一个“QUIT”命令给服务器。
$ echo "QUIT" | nc host.example.com 20-30
SSH-1.99-OpenSSH_3.6.1p2
Protocol mismatch.
220 host.example.com IMS SMTP Receiver Version 0.84 Ready

#### 五、例子

1、建立到主机host.example.com端口42的TCP连接，本地源端口为31337，超时时间为5秒
$ nc -p 31337 -w 5 host.example.com 42

2、建立到主机host.example.com端口53的UDP连接
$ nc -u host.example.com 53

3、建立到主机host.example.com端口42的TCP连接，本地源IP地址为10.1.2.3 
$ nc -s 10.1.2.3 host.example.com 42

4、建立和监听一个UNIX域的流套接字
$ nc -lU /var/tmp/dsocket

5、通过HTPP代理10.2.3.4的8080端口连接主机host.example.com的42号端口。这个例子也可以被ssh用到，查询ssh_config的ProxyCommand命令获取更多信息
$ nc -x10.2.3.4:8080 -Xconnect host.example.com 42

6、同样的例子，该例子使能了代理认证，如果代理需要，使用了用户名'ruser'
$ nc -x10.2.3.4:8080 -Xconnect -Pruser host.example.com 42

#### 六、远程监控案例

案例说明：
1、服务器IP地址：23.65.55.252，服务器上运行了一个服务，该服务会将访问该服务器http服务（80端口）的用户IP写到一个文本文件中，文件名为http_rec.txt
2、用户想看每天有哪些IP地址访问，需要用ssh登陆到服务器上，查看文件http_rec.txt增加了哪些IP，比较麻烦
3、需求：用户在客户端能够实时监控远程服务器上文件http_rec.txt的改变

实现步骤：
1、服务器端
$tail -f http_rec.txt | nc 23.65.55.251 5901

2、客户端在5901端口启动监听，一旦有新的IP地址访问服务器，会显示在客户端终端上
$nc -l 23.65.55.251 5901 //23.65.55.251为本机IP地址，不能写localhost

也可以将监控内容写入客户端某个文件
$nc -l 23.65.55.251 5901 > ~/tmp/http_rec.txt &

搞定。