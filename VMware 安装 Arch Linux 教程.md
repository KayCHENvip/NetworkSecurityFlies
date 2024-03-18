# VMware 安装 Arch Linux 教程（2021.7）



#### 文章目录

- [一、创建虚拟机](https://blog.csdn.net/takashi77/article/details/118927109#_14)
- [二、开始安装](https://blog.csdn.net/takashi77/article/details/118927109#_22)
- - [2.1 开启虚拟机](https://blog.csdn.net/takashi77/article/details/118927109#21_31)
  - [2.2 更新时间](https://blog.csdn.net/takashi77/article/details/118927109#22_38)
  - [2.3 磁盘分区](https://blog.csdn.net/takashi77/article/details/118927109#23_48)
  - [2.4 初始化分区](https://blog.csdn.net/takashi77/article/details/118927109#24__68)
  - [2.5 开始安装系统](https://blog.csdn.net/takashi77/article/details/118927109#25__96)
  - [2.6 配置 locale](https://blog.csdn.net/takashi77/article/details/118927109#26_locale_123)
- [三、设置主机](https://blog.csdn.net/takashi77/article/details/118927109#_153)
- - [3.1 网络配置](https://blog.csdn.net/takashi77/article/details/118927109#31__170)
  - [3.2 设置 root 密码](https://blog.csdn.net/takashi77/article/details/118927109#32_root_178)
  - [3.3 创建新用户](https://blog.csdn.net/takashi77/article/details/118927109#33__182)
  - [3.4 修改新用户的密码：](https://blog.csdn.net/takashi77/article/details/118927109#34__192)
  - [3.5 安装 grub](https://blog.csdn.net/takashi77/article/details/118927109#35_grub_203)
  - [3.6 退出环境重启系统](https://blog.csdn.net/takashi77/article/details/118927109#36__227)





首先下载 Arch 镜像：https://archlinux.org/download/
进去后拉到下面可以选择不同国家的下载链接

值得注意的是这里有一个`版本号`下面新建虚拟机时会用到

![img](https://img-blog.csdnimg.cn/20210722083922382.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rha2FzaGk3Nw==,size_16,color_FFFFFF,t_70)





进去后下载`iso`后缀的镜像文件

![img](https://img-blog.csdnimg.cn/20210722083115803.png)





## 一、创建虚拟机



使用 [VMware](https://so.csdn.net/so/search?q=VMware&spm=1001.2101.3001.7020) 创建新的虚拟机导入刚刚下载好的镜像文件，注意由于 VMware 里面没有 Arch Linux 的选项，这里的版本号根据上面下载时的版本来选择，操作系统就选择 Linux

![img](https://img-blog.csdnimg.cn/20210722083719169.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rha2FzaGk3Nw==,size_16,color_FFFFFF,t_70)

然后是自己给虚拟机起一个名字和指定安装位置，我这里安装到了 G 盘（根据自己的实际情况来定），然后就可以下一步了

![img](https://img-blog.csdnimg.cn/20210722084239101.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rha2FzaGk3Nw==,size_16,color_FFFFFF,t_70)





然后是分配给虚拟机分配内存，想给虚拟机多一点内存空间就可以大一点，下面的默认就行；然后下一步就可以看到虚拟机信息并可以点击完成创建

![img](https://img-blog.csdnimg.cn/20210722084352680.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rha2FzaGk3Nw==,size_16,color_FFFFFF,t_70)





## 二、开始安装



这个时候先别开启虚拟机，先到`虚拟机->设置->选项->高级`中选择”选择通过 EFI 而非 BIOS 引导 (B) 选项（因为我这里是 EFI 启动，当然也可以选择 BIOS 启动方式），点击确定



注意：若是 VMware workstation player 来启动的话需要在上面自行设置的安装目录下

1. 打开`arch.vmx`文件，
2. 在文件末尾添加`firmware = "efi"`进行设置为 EFI 启动





![img](https://img-blog.csdnimg.cn/20210722085943554.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rha2FzaGk3Nw==,size_16,color_FFFFFF,t_70)





### 2.1 开启虚拟机



进入开启界面选择第一个进入

![img](https://img-blog.csdnimg.cn/20210722090139688.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rha2FzaGk3Nw==,size_16,color_FFFFFF,t_70)

由于安装镜像时已经预配置好了网络所以我这里是不用进行配置的，你们可以 ping 一下百度看网络是否可通





ping www.baidu.com
关于无线网络连不上网可以看一下官方文档：https://wiki.archlinux.org/title/Network_configuration/Wireless#Rfkill_caveat



### 2.2 更新时间



1. 查看时间是否准确

```
timedatectl status
```

2. 时间不正确可通过 ntp 校准时间

```
timedatectl set-ntp true
```



### 2.3 磁盘分区



可以使用`lsblk`查看当前分区情况，下面是磁盘未分区前的

![img](https://img-blog.csdnimg.cn/20210723084430713.png)

我们需要分出三个区，用于挂载 FEI 启动分区的 sda1（官方建议最少 512M），用于储存的分区 sda2；用于系统缓存的分区 sda3





进入分区系统

```
cfdisk /dev/sda
```



选择`gpt`方式进行分区

![img](https://img-blog.csdnimg.cn/20210723085546779.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rha2FzaGk3Nw==,size_16,color_FFFFFF,t_70)

然后通过左右键选中`New`回车，然后输入分配给该分区的大小，第一个我们作为 EFI 启动分区，分配 512M 或者更大也可以；然后依次创建三个分区，第二个储存区可以考虑给 6.5G，第三个缓存可以给个 1G 或者更大的

![img](https://img-blog.csdnimg.cn/20210723085743975.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rha2FzaGk3Nw==,size_16,color_FFFFFF,t_70)

![img](https://img-blog.csdnimg.cn/20210723090151536.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rha2FzaGk3Nw==,size_16,color_FFFFFF,t_70)

左右键选择`type`回车，然后找到`EFI System`，选中回车；这样第一个启动盘的 EFI 分区就弄好了；接下来弄存储分区 sda2，然后 sda2 的分区类型为 Linux root （x86-64），第三个为缓存分区 sda3，它的类型为 Linux swap；最后如下面这张图所示；

![img](https://img-blog.csdnimg.cn/20210723201713108.png)

`注意注意！！！`这个时候不要直接退出，因为这个时候只是虚拟分区并没有真正写入到内存中；我们要先左右键选中 Write 写入输入 yes 回车；最后才能 Quit

![img](https://img-blog.csdnimg.cn/20210723202044693.png)

退出后我们可以使用 lsblk 查看已经被我们分区后的磁盘

![img](https://img-blog.csdnimg.cn/202107232022319.png)





### 2.4 初始化分区



分区完成后，需要对分区做格式化处理，由于这里使用了 EFI 分区，因为 EFI 分区需要 FAT32 文件格式（如果是在真机上已安装有 Windows 的情况下安装 Linux 成双系统，且以 EFI 引导系统，则 EFI 分区不需要再次格式化），所以需要将其格式化为 FAT32 格式；根分区格式化为 ext4 格式；



```
mkfs.fat -F32 /dev/sda1
mkfs.ext4 /dev/sda2
mkswap /dev/sda3 -L Swap
swapon /dev/sda3
```



挂载



```
mount /dev/sda2 /mnt
mkdir -p /mnt/boot/EFI
mount /dev/sda1 /mnt/boot/EFI
```



选择镜像源
这里使用一个 nano 命令



```
nano /etc/pacman.d/mirrorlist
```



进入 nano 页面后，按 F6 搜索 “China” 以寻找中国镜像源，如果觉得跳出来镜像源的不是你想要的，你可以按 F6+Enter 继续找
选好之后，按方向键定到 Server 那一行，然后按 Ctrl+K 剪切该行，按方向键将光标拖到最顶端，按 Ctrl+U 粘贴，然后按 Ctrl+O 保存，保存后按回车再按 Ctrl+X 退出，`或者`按照下面的直接添加



```
使用vim打开然后在里面列表最上面添加中国境内的源
Server = http://mirrors.aliyun.com/archlinux/$repo/os/$arch
```



### 2.5 开始安装系统



```
pacstrap -i /mnt base base-devel linux vim dhcpcd net-tools

这是为/mnt下安装一些环境用于待会创建用户所使用的，包括一些基本环境以及网络相关的dhcpcd和net-tools
```



等待基本系统安装完成后，用以下命令生成 fstab 文件 (用 -U 或 -L 选项设置 UUID 或卷标)：



```
genfstab -U /mnt >> /mnt/etc/fstab

生成后可以使用cat /mnt/etc/fstab 查看是否生成了磁盘分区相关的内容（可忽略）
```



切换用户



```
arch-chroot /mnt

切换成功后，root颜色转为灰色
```





![img](https://img-blog.csdnimg.cn/20210723204826726.png)





chroot 之后，当前目录就变成为 / 。此步会自动进行创建初始的 ramdisk 环境，但是如果以后更改了内核配置了的话，最好使用一下命令再重新生成 ramdisk 环境：



```
mkinitcpio -p linux
```



### 2.6 配置 locale



这一步对使用地区和语言等进行配置。在 / etc/locale.gen 文件中进行配置，locale.gen 是一个仅包含注释文档的文本文件。指定需要的本地化类型，只需移除对应行前面的注释符号（＃）即可，使用下面命令打开 locale.gen 文件



```
vim /etc/locale.gen
```



然后找到下面 3 项，去掉每项前面的 #即可（可在指令界面使用:/en_US.UTF-8 UTF-8 进行查找，不匹配按 N 键为下一个匹配项）

![img](https://img-blog.csdnimg.cn/20210723205739669.png)





```
en_US.UTF-8 UTF-8
zh_CN.UTF-8 UTF-8
zh_TW.UTF-8 UTF-8
```



locale-gen`生成Locale信息，并使用`locale -a` 列出所有启用的 Locale：



```
locale-gen
locale -a
```



最后创建 locale.conf 文件，并提交所要使用的本地化选项，然后使用 locale 命令显示当前正在使用的 Locale 和相关的环境变量：



```
echo LANG=en_US.UTF-8 > /etc/locale.conf
locale
```



/etc/locale.conf 用来配置整个系统所使用的 Loacle，而这也可以由用户通过用户自己的 ~/.config/locale.conf （~ 表示当前用户的 Home 目录）来覆盖整个系统的 Locale 配置。



提示：建立 /etc/skel/.config/locale.conf 文件，可以在新用户的建立（新用户的建立见下文）且同时创建用户主目录（useradd -m）时，自动应用其中的 Locale（会将此文件复制到新建用户的 ~/.config/locale.conf 中）。



注意：不推荐此时设置任何中文 locale，因为这样做可能会导致 tty 显示乱码。



## 三、设置主机



要设置主机名，创建 /etc/hostname 文件并将主机名写入该文件即可。我的主机名为 arch：



```
echo arch > /etc/hostname
```



然后配置主机名对应的 IP 到 /etc/hosts 中：



```
vim /etc/hosts
```



把下面的 ip 地址依次填入文件中，其中最后一行中的 arch 为用户名，填上面自己设置的用户名；最后保存退出



```
127.0.0.1    localhost.localdomain    localhost
::1          localhost.localdomain    localhost
127.0.1.1    arch.localdomain    arch 
```



### 3.1 网络配置



```
pacman -S dhcpcd
systemctl enable dhcpcd.service

使用无线网络的话，则需安装以下几个软件包（未验证）
pacman -S iw wpa_supplicant dialog	（有线网络忽略这行）
```



### 3.2 设置 root 密码



```
passwd
```



### 3.3 创建新用户



因为使用 root 用户登陆后，root 用户拥有系统的所有操作权限，这样对系统的操作非常不安全（如一不小心将系统文件删除了，怎么办？），所以需要新建一个普通用户，让其对系统的操作受到一定限制，使用下面命令新建用户 free：



```
useradd -m -G wheel -s /bin/bash free
```



-m：创建用户主目录（/home/[用户名]）
-G：用户要加入的附加组列表；此处将用户加到 wheel 组中，之后可以给这个组执行 sudo 命令的权限
-s：指定了用户默认登录 shell 的路径，此处设置为 bash 的路径
更多创建新用户的使用请查看 Arch Linux WiKi：[Users and groups（简体中文）](https://wiki.archlinux.org/title/Users_and_groups_(简体中文))。



### 3.4 修改新用户的密码：



```
passwd free
```



为了让我们的普通用户也能使用 sudo 权限，需要给 wheel 中的用户赋予权限,
使用上面命令打开 sudoers 文件后，删除 wheel 组前面的注释（#）即可：



```
visudo
```





![img](https://img-blog.csdnimg.cn/2021072321143921.png)





### 3.5 安装 grub



grub 是一个启动引导器，同时支持 EFI 和 BIOS 方式的启动。若使用的 UEFI 方式引导系统，则还需要安装 efibootmgr，如果是双系统的话，还需要安装 os-prober，且如果使用 Intel CPU 的话，则需要安装 intel-ucode 并启用[因特尔微码更新](https://wiki.archlinux.org/title/Microcode_(简体中文)



因为我们使用的是虚拟机和 UEFI 引导方式，因此只需要安装 grub 和 efibootmgr：



```
pacman -S grub efibootmgr
```



然后，还需要将其安装到 EFI 分区当中：



```
grub-install --recheck /dev/sda
```



注意：此处的 /dev/sda 后没有数字。



若提示 error:cannot find EFI directory，则说明 EFI 文件夹的路径不正确，找不到 EFI 文件夹的位置，此时就需要在上面命令中加入 efi-directory 参数指定安装路径：



```
grub-install --recheck /dev/sda --efi-directory=/boot	(注意这是上一句命令操作成功的不需要操作这一条)
```



最后还需要生成一个 grub 的配置文件：



```
grub-mkconfig -o /boot/grub/grub.cfg
```



此时已经安装完毕



### 3.6 退出环境重启系统



```
exit
umount -R /mnt
reboot
```



重启后可使用刚才添加的用户和密码进行登录



安装完进入系统以后以后可能会出现`网络无法访问`的情况，即当 ping 百度出现下面错误



```
ping: www.baidu.com: Temporary failure in name resolution
```



解决办法：



```
vim /etc/resolv.conf 
在里面添加`nameserver 8.8.8.8`
```





参考链接

https://www.cnblogs.com/freerqy/p/8508395.html
[https://wiki.archlinux.org/title/Installation_guide_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)](https://wiki.archlinux.org/title/Installation_guide_(简体中文))