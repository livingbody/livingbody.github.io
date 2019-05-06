---
layout: post
title:  "linux shell 备份文件"
date:   2019-05-06 23:54:00
categories: linux 
tags: linux 
author: livingbody
---

# linux  shell 备份文件

**很久没来blog了，今天特别感谢xudailong，csdn留言得到回复，修改了blog分页配置**

<u>下面为备份日志至ftp，加上定时则更佳</u>



```bash
#!/bin/bash
#scripts for dirbakup and upload to ftp server
#author by livingbody


bakdir=log
dd=$(date +%F)

cd /var
tar -zcf ${bakdir}_${dd}.tar.gz ${bakdir}
sleep 1
ftp -n <<-EOF
open 192.168.43.200
user root root
put ${bakdir}_${dd}.tar.gz
bye
EOF
rm -rf ${bakdir}_${dd}.tar.gz



```

