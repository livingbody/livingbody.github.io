---
layout: post
title:  "E Sub-process usrbindpkg returned an error code (1)错误解决]"
date:   2018-02-28 10:00:00
categories: linux
tags: linux
author: livingbody
---

# E: Sub-process /usr/bin/dpkg returned an error code (1)错误解决](https://www.cnblogs.com/nkh222/p/8126455.html) 		

## 在用apt－get安装软件时出现了类似于

`install-info:  No dir file specified; try --help for more information.dpkg：处理 gettext  (--configure)时出错： 子进程 post-installation script 返回了错误号 1 在处理时有错误发生：`
`findutils`
`E: Sub-process /usr/bin/dpkg returned an error code (1)`

## 办法如下：

`1.$ sudo mv /var/lib/dpkg/info /var/lib/dpkg/info_old //现将info文件夹更名`
`2.$ sudo mkdir /var/lib/dpkg/info //再新建一个新的info文件夹`
`3.$ sudo apt-get update,再​$sudoapt-get -f install //不用解释了吧`
`4.$ sudo mv /var/lib/dpkg/info/* /var/lib/dpkg/info_old //执行完上一步操作后会在新的info文件夹下生成一些文件，现将这些文件全部移到info_old文件夹下`
`5.$ sudo rm -rf /var/lib/dpkg/info //把自己新建的info文件夹删掉`
`6.$ sudo mv /var/lib/dpkg/info_old /var/lib/dpkg/info //把以前的info文件夹重新改回名字`
`到此问题顺利解决`


