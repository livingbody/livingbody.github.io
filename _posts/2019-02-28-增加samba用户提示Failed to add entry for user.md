---
layout: post
title:  "增加samba用户提示Failed to add entry for user"
date:   2018-02-28 10:00:00
categories: linux
tags: samba
author: livingbody
---







# 增加samba用户提示Failed to add entry for user



## 现象

```shell
[root@ubuntu ~]# smbpasswd -a test
New SMB password:
Retype new SMB password:
Failed to add entry for user test.
```



## 解决办法:

这是因为没有加相应的系统账号，所以会提示Failed to add entry for user的错误，只需增加相应的系统账号test就可以了:

```shell
[root@ubuntu ~]# groupadd test -g 6000
[root@ubuntu ~]# useradd test -u 6000 -g 6000 -s /sbin/nologin -d /dev/null
```

这时就可以用smbpasswd -a test增加test这个samba账号了!为了增加系统的安全性，所以加的系统账号不要给shell它，也不给它指定目录，到时在/home目录给test账号建个文件夹，该文件夹只有test有读写权限即可!

```shell
[root@ubuntu ~]# mkdir /home/test
[root@ubuntu ~]# chown -R test:test /home/test
```



