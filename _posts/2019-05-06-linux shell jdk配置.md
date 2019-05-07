---
layout: post
title:  "linux shell jdk配置"
categories: linux 
tags: linux 
author: livingbody
---

# linux shell jdk配置





```bash
#!/bin/bash
#scripts for dirbakup and upload to ftp server
#author by livingbody


cat >> /etc/profile << EOF
export JAVA_HOME=/usr/local/jdk1.8.0_162
export CLASSPATH=.:$JAVA_HOME/jre/lib/rt.jar:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export PATH=$JAVA_HOME/bin:$PATH

EOF
source /etc/profile



```

