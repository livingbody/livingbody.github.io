---
layout: post
title:    Windows下文本上传linux脚本出现错误 
categories: linux
tags: linux shell
author: livingbody
---







# Windows下文本上传linux脚本出现错误

经查找资料发现

```bash
1. 在windows下的文本文件的每一行结尾，都有一个回车('\n')和换行('\r')
2. 在linux下的文本文件的每一行结尾，只有一个回车('\n');
3. 在Mac下的文本文件的每一行结尾，只有一个换行('\r');
```

 


   解决该问题的方法

```bash
dos2unix filename   #  直接转换成unix格式 此方法在终端下可用。由于本人使用为ubuntu系统，没有该命令。
todos filename    # ubuntu 终端使用, 如果未安装 sudo apt-get install tofrodos    todos(相当于unix2dos)，和fromdos(相当于dos2unix)
cat   原文件| tr -d "^M" > 新文件
mv 新文件 原文件
或者：
sed 's/^M//原文件>新文件
mv 新文件 原文件
或者：
dos2unix test.sh
Linux命令行下
或者：
$ vim log.txt 
    :set fileformat=unix
    :wq
```



---------------------
