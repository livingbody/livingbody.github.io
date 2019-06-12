---
layout: post
title:    kickstart模式实现批量安装redhat7.x系统
categories:  kickstart
tags: redhat linux
author: livingbody
---







# kickstart模式实现批量安装redhat7.x系统



## 0.selinux、防火墙

```bash
systemctl status firewalld.service
systemctl stop firewalld.service
getenforce
vim /etc/selinux/config

# This file controls the state of SELinux on the system.
# SELINUX= can take one of these three values:
#     enforcing - SELinux security policy is enforced.
#     permissive - SELinux prints warnings instead of enforcing.
#     disabled - No SELinux policy is loaded.
SELINUX=disabled
#SELINUX=enforcing
# SELINUXTYPE= can take one of three values:
#     targeted - Targeted processes are protected,
#     minimum - Modification of targeted policy. Only selected processes are protected. 
#     mls - Multi Level Security protection.
SELINUXTYPE=targeted 
```

1.dhcp

```bash
yum install dhcp -y
systemctl start dhcpd.service
systemctl status dhcpd.service

vim /etc/dhcp/dhcpd.conf
#
# DHCP Server Configuration file.
#   see /usr/share/doc/dhcp*/dhcpd.conf.example
#   see dhcpd.conf(5) man page
#
ddns-update-style none;

allow booting;

allow bootp;

filename "/pxelinux.0";

next-server 172.16.1.201;

subnet 172.16.1.0 netmask 255.255.255.0 {

	option subnet-mask  255.255.255.0;

	option broadcast-address 172.16.1.255;

	default-lease-time 3600;

	range dynamic-bootp 172.16.1.100 172.16.1.199;

}

############################
```

## 2.tftp

```bash
 yum install tftp-server -y
 systemctl start tftp.socket
 systemctl status tftp.socket
 tftp根目录 /var/lib/tftpboot/
```

## 3.获取pxelinux.0系统

```bash
 yum install -y syslinux
cp /usr/share/syslinux/pxelinux.0  /var/lib/tftpboot/
```

## 4.挂载系统

### 4.1实际生产环境镜像复制到硬盘速度更快

```bash
mkdir -p /var/www/html/CentOS7
mount /dev/cdrom /var/www/html/CentOS7
```

### 4.2将镜像中的相关文件复制到tftp根目录

```bash
cp -a /var/www/html/CentOS7/isolinux/* /var/lib/tftpboot/
mkdir -p /var/lib/tftpboot/pxelinux.cfg
cp /var/www/html/CentOS7/isolinux/isolinux.cfg /var/lib/tftpboot/pxelinux.cfg/default
```

## 5.修改default配置文件实现通过网络安装操作系统

```bash
/var/lib/tftpboot/pxelinux.cfg
default vesamenu.c32
timeout 600

display boot.msg

# Clear the screen when exiting the menu, instead of leaving the menu displayed.
# For vesamenu, this means the graphical background is still displayed without
# the menu itself for as long as the screen remains in graphics mode.
menu clear
menu background splash.png
menu title Red Hat Enterprise Linux 7.6
menu vshift 8
menu rows 18
menu margin 8
#menu hidden
menu helpmsgrow 15
menu tabmsgrow 13

# Border Area
menu color border * #00000000 #00000000 none

# Selected item
menu color sel 0 #ffffffff #00000000 none

# Title bar
menu color title 0 #ff7ba3d0 #00000000 none

# Press [Tab] message
menu color tabmsg 0 #ff3a6496 #00000000 none

# Unselected menu item
menu color unsel 0 #84b8ffff #00000000 none

# Selected hotkey
menu color hotsel 0 #84b8ffff #00000000 none

# Unselected hotkey
menu color hotkey 0 #ffffffff #00000000 none

# Help text
menu color help 0 #ffffffff #00000000 none

# A scrollbar of some type? Not sure.
menu color scrollbar 0 #ffffffff #ff355594 none

# Timeout msg
menu color timeout 0 #ffffffff #00000000 none
menu color timeout_msg 0 #ffffffff #00000000 none

# Command prompt text
menu color cmdmark 0 #84b8ffff #00000000 none
menu color cmdline 0 #ffffffff #00000000 none

# Do not display the actual menu unless the user presses a key. All that is displayed is a timeout message.

menu tabmsg Press Tab for full configuration options on menu items.

menu separator # insert an empty line
menu separator # insert an empty line

label linux
  menu label ^Install Red Hat Enterprise Linux 7.6
  kernel vmlinuz
#  append initrd=initrd.img inst.stage2=hd:LABEL=RHEL-7.6\x20Server.x86_64 quiet
	append initrd=initrd.img ks=http://172.16.1.201/ks_config/CentOS7-ks.cfg

#label check
#  menu label Test this ^media & install Red Hat Enterprise Linux 7.6
#  menu default
#  kernel vmlinuz
#  append initrd=initrd.img inst.stage2=hd:LABEL=RHEL-7.6\x20Server.x86_64 rd.live.check quiet

menu separator # insert an empty line

# utilities submenu
menu begin ^Troubleshooting
  menu title Troubleshooting

label vesa
  menu indent count 5
  menu label Install Red Hat Enterprise Linux 7.6 in ^basic graphics mode
  text help
	Try this option out if you're having trouble installing
	Red Hat Enterprise Linux 7.6.
  endtext
  kernel vmlinuz
  append initrd=initrd.img inst.stage2=hd:LABEL=RHEL-7.6\x20Server.x86_64 xdriver=vesa nomodeset quiet

label rescue
  menu indent count 5
  menu label ^Rescue a Red Hat Enterprise Linux system
  text help
	If the system will not boot, this lets you access files
	and edit config files to try to get it booting again.
  endtext
  kernel vmlinuz
  append initrd=initrd.img inst.stage2=hd:LABEL=RHEL-7.6\x20Server.x86_64 rescue quiet

label memtest
  menu label Run a ^memory test
  text help
	If your system is having issues, a problem with your
	system's memory may be the cause. Use this utility to
	see if the memory is working correctly.
  endtext
  kernel memtest

menu separator # insert an empty line

label local
  menu label Boot from ^local drive
  localboot 0xffff

menu separator # insert an empty line
menu separator # insert an empty line

label returntomain
  menu label Return to ^main menu
  menu exit

menu end
```

## 6.配置httpd服务

```bash
安装apache服务

[root@kickstart tftpboot]# yum install httpd -y
启动apache服务

[root@kickstart tftpboot]# systemctl start httpd.service
[root@kickstart tftpboot]# systemctl status httpd.service
● httpd.service - The Apache HTTP Server
通过浏览器访问，使用curl命令镜像访问。

http://10.0.0.201/CentOS7/
curl http://172.16.1.201/CentOS7/
```

## 7.配置ks文件

```bash
/var/www/html/ks_config/CentOS7-ks.cfg
install   #告知安装程序，这是一次全新安装，而不是升级
url --url="http://172.16.1.201/CentOS7/"  #通过http下载安装镜像
text     #以文本格式安装
lang en_US.UTF-8   #设置字符集格式
keyboard us  #设置键盘类型
zerombr   #清除mbr引导
bootloader --location=mbr --driveorder=sda --append="crashkernel=auto rhgb quiet"    #指定引导记录被写入的位置
network  --bootproto=static --device=ens38 --gateway=10.0.0.254 --ip=10.0.0.202 --nameserver=223.5.5.5 --netmask=255.255.255.0 --activate  #配置eth0网卡
network  --bootproto=static --device=ens33 --ip=172.16.1.202 --netmask=255.255.255.0 --activate   #配置eth1网卡
network  --hostname=Cobbler  #设置主机名
network --bootproto=dhcp --device=ens33 --onboot=yes --noipv6 --hostname=CentOS7
timezone --utc Asia/Shanghai  #可以使用dhcp方式设置网络
authconfig --enableshadow --passalgo=sha512  #设置密码格式
rootpw  "1qaz@wsx"     #明文密码
clearpart --all --initlabel  #清空分区
part /boot --fstype xfs --size 1024   #/boot分区
part swap --size 1024                    #swap分区
part / --fstype xfs --size 1 --grow   #/分区
firstboot --disable       #负责协助配置redhat一些重要的信息
selinux --disabled        #关闭selinux
firewall --disabled       #关闭防火墙
logging --level=info      #设置日志级别
reboot                       #安装完成重启

%packages #包组段   @表示包组
@^minimal
@compat-libraries
@debugging
@development
tree
nmap
sysstat
lrzsz
dos2unix
telnet
wget
vim
bash-completion
%end

%post #脚本段，可以放脚本或命令
systemctl disable postfix.service   #关闭邮件服务开机自启动
%end
```

8.备注

```bash
linux kickstart文件里rootpw密码可以使用明文，也可以使用加密过的值，这里主要介绍下三种加密方法：md5、sha256、sha512
使用明文的方法
rootpw "password"
使用加密的方法
rootpw --iscrypted password_hash
authconfig --enableshadow --enablemd5 (--passalgo=sha256 or --passalgo=sha512)
1、md5加密
使用openssl passwd命令：

# openssl passwd -1 "password"
$1$uMOl6YMI$7AAO8YG7l37ipRXCmmame.
使用grub-crypt命令，会提示输出密码：

# grub-crypt --md5
Password: 
Retype password: 
$1$Y9TR8PpY$qm1VzsjKzbXtYInyAQLG70
使用python，同样也会提示输出密码：

# echo 'import crypt,getpass; print crypt.crypt(getpass.getpass(), "$1$8_CHARACTER_SALT_HERE")' | python -
Password: 
$1$8_CHARAC$GVWpvO3Hu009C37IYF41L0
2、sha256加密
使用grub-crypt命令，会提示输出密码：

# grub-crypt --sha-256
Password: 
Retype password: 
$5$NSEqzlxQFNE998rG$gDTEQsndo1pQ9/2.bj1knNNqQ0tQgzKH4bdzEjinHKC
使用python，提示输入密码：

# echo 'import crypt,getpass; print crypt.crypt(getpass.getpass(), "$5$16_CHARACTER_SALT_HERE")' | python -
$5$16_CHARACTER_SAL$sc08xCjatZRZPSxgCvHe2.RN7ocYGCrJZo6JzcOMtk5
3、sha512加密
使用grub-crypt命令，会提示输出密码：

# grub-crypt --sha-512
Password: 
Retype password:
$6$twuCoL0kTI5ScTbr$GyUJymp1wU0ouFQFiWXoOfl2i.2G5E5wh3tqdprny4avv9kJWc3MdLR/GB9YbfKB1Kx9no9wpO8YcX4d28Mrz.
使用python，提示输入密码：

# echo 'import crypt,getpass; print crypt.crypt(getpass.getpass(), "$6$16_CHARACTER_SALT_HERE")' | python -
$6$16_CHARACTER_SAL$ykxE75iUZiphsLz40.oQAi7QIM4meq41EYYvQ66JkbODcvIrGIeRxF7dzpfvnk20ztzE3GY359DSSNQuPQdun.

```



---------------------
