# 【内网渗透】mimikatz 基本使用

## 前言



本文主要是记录内网渗透神器`mimikatz`的主要使用方法，作为今后在渗透过程中的一个简单手册。



## 1. 工具简介



项目地址：https://github.com/gentilkiwi/mimikatz/



作者一直在更新这个项目，截止今日，最近一次更新 2021 年 8 月 10 日。



mimikatz 可以从内存中提取明文密码、哈希、PIN 码和 kerberos 票证。 mimikatz 还可以执行哈希传递、票证传递或构建黄金票证。



功能模块命令如下：



```
cls：       清屏
standard：  标准模块，基本命令
crypto：    加密相关模块
sekurlsa：  与证书相关的模块
kerberos：  kerberos模块
privilege： 提权相关模块
process：   进程相关模块
serivce：   服务相关模块
lsadump：   LsaDump模块
ts：        终端服务器模块
event：     事件模块
misc：      杂项模块
token：     令牌操作模块
vault：     Windows 、证书模块
minesweeper：Mine Sweeper模块
net：
dpapi：     DPAPI模块（通过API或RAW访问）[数据保护应用程序编程接口]
busylight： BusyLight Module
sysenv：    系统环境值模块
sid：       安全标识符模块
iis：       IIS XML配置模块
rpc：       mimikatz的RPC控制
sr98：      用于SR98设备和T5577目标的RF模块
rdm：       RDM（830AL）器件的射频模块
acr：       ACR模块
version：   查看版本
exit：      退出
```



## 2. 提升权限 privilege::debug



通过 debug 获得 mimikatz 程序的特殊操作。



调试权限允许某人调试他们本来无法访问的进程。例如，作为用户运行的进程在其令牌上启用了调试权限，可以调试作为本地系统运行的服务。



![img](http://upload-images.jianshu.io/upload_images/18738459-79aaea4e26e0b1ec.png)

image



当出现`ERROR kuhl_m_privilege_simple ; RtlAdjustPrivilege (20) c0000061`时，表示客户端未持有所需的权限，即不是管理员。



![img](http://upload-images.jianshu.io/upload_images/18738459-f0a8b008f4ca2b2e.png)

image



## 3. 抓取明文密码 sekurlsa::logonpasswords



在 windows2012 以上的系统不能直接获取明文密码了，需要配置相关注册表等操作。



```
mimikatz # privilege::debug
mimikatz # sekurlsa::logonpasswords

或者直接运行：
mimikatz.exe "privilege::debug" "sekurlsa::logonpasswords"
```



![img](http://upload-images.jianshu.io/upload_images/18738459-69f66352b8fa8374.png)

image



**分析命令执行后的内容**：



前面几行的`Authentication` ... `SID`等值就是一些基本信息。



**msv**：这项是账户对应密码的各种加密协议的密文，可以看到有 LM、NTLM 和 SHA1 加密的密文。



**tspkg，wdigest，kerberos**：这个就是账户对应的明文密码了。有的时候这三个对应的也不是全部都是一样的，需要看服务器是什么角色。



**SSP**：是在该机器上，最近登录到其他 RDP 终端的账户和密码。



## 4. sekurlsa 模块 获取密码信息



Mimikatz 提取用户凭证功能，其主要集中在 sekurlsa 模块，该模块又包含很多子模块，如 msv，wdigest，kerberos 等。如上面演示的抓取明文密码的`sekurlsa::logonpasswords`模块。



使用这些子模块可以提取相应的用户凭证，如



`sekurlsa::msv`提取 ntlm hash 凭证（对应上面截图的 msv 部分）；



`sekurlsa::wdigest`提取用户密码明文（对应上面截图的 wdigest 部分）；



`sekurlsa::kerberos`提取域账户凭证。



### 4.1 procdump + mimikatz 加载 dmp 文件，并导出其中的明文密码



procdump 工具，可以将`lsass.exe`进程的内存文件导出来，由 mimikatz 对导出的内存文件进行分析，从而获取密码。



新版`procdump v10.1` 使用时存在错误：https://docs.microsoft.com/en-us/answers/questions/500002/new-procdump-not-working-in-window-server-2016-160.html



开发者在下面也进行了回复，ProcDump v10.1 添加了对 IPT 流的支持——操作系统 / 调试器应该支持 Winv8.1+ 的 IPT——但它似乎甚至不在 Win10-RS1/Win2016 服务器中。



修复程序正在开发中。



```
[15:44:40] Dump 1 initiated: c:\temp\cmd.exe_210803_154440.dmp
[15:44:40] Dump 1 error: Error writing dump file: 0x80070057
The parameter is incorrect. (0x80070057, -2147024809)

[15:44:40] Dump count reached. 
```



![img](http://upload-images.jianshu.io/upload_images/18738459-4c9a342e8405bca6.png)

image



这里我使用`procdump v8.0`



关注公众号 “信安文摘”，回复“procdump” 下载。





管理员运行工具，导出为 lsass.dump 文件：



```
procdump64.exe -accepteula -ma lsass.exe lsass.dmp
```



![img](http://upload-images.jianshu.io/upload_images/18738459-c9452017af0952fb.png)

image



将 lsass.dmp 放在 mimikatz 同一目录，读取密码文件：



```
sekurlsa::minidump lsass.dmp
sekurlsa::logonpasswords full
```



![img](http://upload-images.jianshu.io/upload_images/18738459-014540e1f9cd714b.png)

image



### 4.2 导出 lsass.exe 进程中所有的票据 sekurlsa::tickets /export



该功能模块导出`lsass.exe`进程中所有的票据，运行该命令会在当前目录生成多个服务的票据文件。



![img](http://upload-images.jianshu.io/upload_images/18738459-63c0340c23b6b82f.png)

image



可以使用这些导出的票据进行票据传递攻击（Pass The Ticket，PTT），对应的另一种攻击方式为哈希传递攻击（Pass The Hash，PTH）。



目前没有遇到这种环境进行学习，该种攻击方法的学习链接如下：票据传递（Pass The Ticket）攻击与利用：https://cloud.tencent.com/developer/article/1752178



之后遇到相关靶场环境（PTH、PTT）再进行学习记录。



## 5. lsadump 模块 读取域控中域成员 Hash



### 5.1 读取所有域用户的哈希 lsadump::lsa /patch



该命令需要在域控机器上执行：



![img](http://upload-images.jianshu.io/upload_images/18738459-044a60b8d3443771.png)

image



### 5.2 查看域内指定用户信息，包括 NTLM 哈希等



该命令需要在域控机器上执行：



```
lsadump::dcsync /domain:god.org /user:ligang
```



![img](http://upload-images.jianshu.io/upload_images/18738459-9348df5b160ea1ab.png)

image



## 总结



文章整理的有点乱，还是对 mimikatz 不够熟悉。mimikatz 的功能远不止这些，包括上面提到的 PTH 和 PTT 攻击，还需要好好深入学习。



## 参考资料



[神器 mimikatz 密码提取工具 - Privilege 模块](https://www.jianshu.com/p/fefcac30af15)



https://blog.csdn.net/weixin_40412037/article/details/113348310



[利用 procdump+mimikatz 读取 windows 系统中的密码](https://blog.csdn.net/LUOBIKUN/article/details/108458804)



https://www.cnblogs.com/-mo-/p/11890232.html



https://www.freebuf.com/articles/web/176796.html