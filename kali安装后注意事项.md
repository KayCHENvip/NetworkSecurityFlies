安装后要注意的：

1、更新源

http://docs.kali.org/general-use/kali-linux-sources-list-repositories

这是官方的源列表。如果因为某些原因，你在安装kali过程中，当被问到“使用网络镜像”时选择了否，可能会导致你的sources.list文件中丢失一些条目，或者因为别的原因导致你使用apt-get命令总是找不到数据包，这时候就要考虑更新下源。

vi /etc/apt/sources.list

可以删除该文件中的所有内容，也可以直接在文前添加新的APT源。

这里我参考官方的更新源以及国内的更新源给出下面的sources.list

\#官网kali源
deb http://http.kali.org/kali kali-rolling main contrib non-free

\#中科大kali源
deb http://mirrors.ustc.edu.cn/kali kali-rolling main non-free contrib
deb-src http://mirrors.ustc.edu.cn/kali kali-rolling main non-free contrib
deb http://mirrors.ustc.edu.cn/kali-security kali-current/updates main contrib non-free
deb-src http://mirrors.ustc.edu.cn/kali-security kali-current/updates main contrib non-free

\#阿里云kali源
deb http://mirrors.aliyun.com/kali kali-rolling main non-free contrib
deb-src http://mirrors.aliyun.com/kali kali-rolling main non-free contrib
deb http://mirrors.aliyun.com/kali-security kali-rolling/updates main contrib non-free
deb-src http://mirrors.aliyun.com/kali-security kali-rolling/updates main contrib non-free

（如果在虚拟机中还没安装open-vm-tools-desktop前不能跟物理机交互的话，就在虚拟机中访问该网页复制上面内容，否则就自己手打吧）

对软件进行一次整体更新：

apt-get update & apt-get upgrade
apt-get dist-upgrade
apt-get clean


2、安装open-vm-tools-desktop

VMware自带的vmware-tools在新版本的Kali中已经没效果，官方建议是安装open-vm-tools-desktop来代替其跟物理机交互。

如果之前不小心安装了vmware-tools，可以输入

vmware-uninstall-tools.pl

回车即可删除vmware-tools。

在有官方的更新源下，使用下面命令安装open-vm-tools-desktop

（如果报破坏了软件包间的依赖关系的错误，是源没设置好，如果访问官方源被禁止连接，再重试几次即可）

apt-get install open-vm-tools-desktop fuse
reboot


3、安装文泉驿字体

Kali选择“简体中文”安装后，在终端等地方发现字体总有重叠，只要安装中文字体即可，这里推荐文泉驿字体。文泉驿是一个以开发开源、免费中文电子资源－－如汉字字体、词库等－－为目标的公益性组织。她的创办宗旨是实现“任何人在任何地方都可以自由使用汉字和汉语进行交流”。

apt-get install ttf-wqy-microhei ttf-wqy-zenhei xfonts-wqy

修改系统字体配置文件

vi /etc/fonts/conf.d/49-sansserif.conf

然后修改倒数第四行的字体为WenQuanYiZen Hei

<?xml version="1.0"?><!DOCTYPE fontconfig SYSTEM "fonts.dtd"><fontconfig><!--If the font still has no generic name, addsans-serif--><match target="pattern"><test qual="all"name="family" compare="not_eq"><string>sans-serif</string></test><test qual="all"name="family" compare="not_eq"><string>serif</string></test><test qual="all"name="family" compare="not_eq"><string>monospace</string></test><edit name="family"mode="append_last"><string>WenQuanYiZen Hei</string></edit></match></fontconfig>

最后重启即可。