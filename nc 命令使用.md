***nc 命令使用***

nc 是 netcat 的简写，有着网络界的瑞士军刀美誉。因为它短小精悍、功能实用，被设计为一个简单、可靠的网络工具。比如大家很熟悉使用 telnet 测试 tcp 端口，而 nc 可以支持测试 linux 的 tcp 和 udp 端口，而且也经常被用于端口扫描，甚至把 nc 作为 server 以 TCP 或 UDP 方式侦听指定端口做简单的模拟测试。

`ncat` 或者说 `nc` 是一款功能类似 `cat` 的工具，但是是用于网络的。它是一款拥有多种功能的 CLI 工具，可以用来在网络上读、写以及重定向数据。 它被设计成可以被脚本或其他程序调用的可靠的后端工具。同时由于它能创建任意所需的连接，因此也是一个很好的网络调试工具。

`ncat`/`nc` 既是一个端口扫描工具，也是一款安全工具，还能是一款监测工具，甚至可以做为一个简单的 TCP 代理。 由于有这么多的功能，它被誉为是网络界的瑞士军刀。 这是每个系统管理员都应该知道并且掌握它。

在大多数 Debian 发行版中，`nc` 是默认可用的，它会在安装系统的过程中自动被安装。 但是在 CentOS 7 / RHEL 7 的最小化安装中，`nc` 并不会默认被安装。 你需要用下列命令手工安装。

```
NAME
       ncat - Concatenate and redirect sockets

SYNOPSIS
       ncat [OPTIONS...] [hostname] [port]

DESCRIPTION
       Ncat is a feature-packed networking utility which reads and writes data across networks
       from the command line. Ncat was written for the Nmap Project and is the culmination of
       the currently splintered family of Netcat incarnations. It is designed to be a reliable
       back-end tool to instantly provide network connectivity to other applications and users.
       Ncat will not only work with IPv4 and IPv6 but provides the user with a virtually
       limitless number of potential uses.

       Among Ncat's vast number of features there is the ability to chain Ncats together;
       redirection of TCP, UDP, and SCTP ports to other sites; SSL support; and proxy
       connections via SOCKS4 or HTTP proxies (with optional proxy authentication as well).
       Some general principles apply to most applications and thus give you the capability of
       instantly adding networking support to software that would normally never support it.

OPTIONS SUMMARY
           Ncat 7.50 ( https://nmap.org/ncat )
           Usage: ncat [options] [hostname] [port]

           Options taking a time assume seconds. Append 'ms' for milliseconds,
           's' for seconds, 'm' for minutes, or 'h' for hours (e.g. 500ms).
             -4                         Use IPv4 only
             -6                         Use IPv6 only
             -U, --unixsock             Use Unix domain sockets only
             -C, --crlf                 Use CRLF for EOL sequence
             -c, --sh-exec <command>    Executes the given command via /bin/sh
             -e, --exec <command>       Executes the given command
                 --lua-exec <filename>  Executes the given Lua script
             -g hop1[,hop2,...]         Loose source routing hop points (8 max)
             -G <n>                     Loose source routing hop pointer (4, 8, 12, ...)
             -m, --max-conns <n>        Maximum <n> simultaneous connections
             -h, --help                 Display this help screen
             -d, --delay <time>         Wait between read/writes
             -o, --output <filename>    Dump session data to a file
             -x, --hex-dump <filename>  Dump session data as hex to a file
             -i, --idle-timeout <time>  Idle read/write timeout
             -p, --source-port port     Specify source port to use
             -s, --source addr          Specify source address to use (doesn't affect -l)
             -l, --listen               Bind and listen for incoming connections
             -k, --keep-open            Accept multiple connections in listen mode
             -n, --nodns                Do not resolve hostnames via DNS
             -t, --telnet               Answer Telnet negotiations
             -u, --udp                  Use UDP instead of default TCP
                 --sctp                 Use SCTP instead of default TCP
             -v, --verbose              Set verbosity level (can be used several times)
             -w, --wait <time>          Connect timeout
             -z                         Zero-I/O mode, report connection status only
                 --append-output        Append rather than clobber specified output files
                 --send-only            Only send data, ignoring received; quit on EOF
                 --recv-only            Only receive data, never send anything
                 --allow                Allow only given hosts to connect to Ncat
                 --allowfile            A file of hosts allowed to connect to Ncat
                 --deny                 Deny given hosts from connecting to Ncat
                 --denyfile             A file of hosts denied from connecting to Ncat
                 --broker               Enable Ncat's connection brokering mode
                 --chat                 Start a simple Ncat chat server
                 --proxy <addr[:port]>  Specify address of host to proxy through
                 --proxy-type <type>    Specify proxy type ("http" or "socks4" or "socks5")
                 --proxy-auth <auth>    Authenticate with HTTP or SOCKS proxy server
                 --ssl                  Connect or listen with SSL
                 --ssl-cert             Specify SSL certificate file (PEM) for listening
                 --ssl-key              Specify SSL private key (PEM) for listening
                 --ssl-verify           Verify trust and domain name of certificates
                 --ssl-trustfile        PEM file containing trusted SSL certificates
                 --ssl-ciphers          Cipherlist containing SSL ciphers to use
                 --version              Display Ncat's version information and exit

           See the ncat(1) manpage for full options, descriptions and usage examples
```

作用：批量端口扫描，可根据扫描主机的配置调整后台扫描进程数量（手动执行后根据统计的执行时间调整脚本中关于进程数量的参数），通过定时任务作为简单的服务监控（可修改脚本添加其他报警功能，例如邮件等）