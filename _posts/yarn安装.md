---
layout: post
title:  "yarn安装"
date:   2018-03-06 10:00:00
categories: yarn
tags: yarn
author: livingbody
---
# yarn安装

(yarn下载地址)[https://yarnpkg.com/zh-Hans/docs/install#windows-stable]

## ubuntu下安装

```bash
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
or
sudo apt-get update && sudo apt-get install yarn
```

## windows下安装

有安装包

## 设置国内源

```bash
npm config set registry https://registry.npm.taobao.org
yarn config set registry https://registry.npm.taobao.org
```
## 查询源地址

```bash
npm config get registry
yarn config get registry
```

## 全局安装其他

```bash
npm install pm2 vue-cli -g
```

```bash
C:\Users\pc\testyarn>npm install pm2 vue-cli -g
npm WARN deprecated coffee-script@1.12.7: CoffeeScript on NPM has moved to "coffeescript" (no hyphen)
C:\Users\pc\AppData\Roaming\npm\pm2-runtime -> C:\Users\pc\AppData\Roaming\npm\node_modules\pm2\bin\pm2-runtime
C:\Users\pc\AppData\Roaming\npm\pm2-dev -> C:\Users\pc\AppData\Roaming\npm\node_modules\pm2\bin\pm2-dev
C:\Users\pc\AppData\Roaming\npm\pm2-docker -> C:\Users\pc\AppData\Roaming\npm\node_modules\pm2\bin\pm2-docker
C:\Users\pc\AppData\Roaming\npm\pm2 -> C:\Users\pc\AppData\Roaming\npm\node_modules\pm2\bin\pm2
C:\Users\pc\AppData\Roaming\npm\vue -> C:\Users\pc\AppData\Roaming\npm\node_modules\vue-cli\bin\vue
C:\Users\pc\AppData\Roaming\npm\vue-list -> C:\Users\pc\AppData\Roaming\npm\node_modules\vue-cli\bin\vue-list
C:\Users\pc\AppData\Roaming\npm\vue-init -> C:\Users\pc\AppData\Roaming\npm\node_modules\vue-cli\bin\vue-init
npm WARN rollback Rolling back readable-stream@2.3.6 failed (this is probably harmless): EPERM: operation not permitted, scandir 'C:\Users\pc\AppData\Roaming\npm\node_modules\pm2\node_modules\fsevents\node_modules'
npm WARN optional SKIPPING OPTIONAL DEPENDENCY: fsevents@1.2.7 (node_modules\pm2\node_modules\fsevents):
npm WARN notsup SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for fsevents@1.2.7: wanted {"os":"darwin","arch":"any"} (current: {"os":"win32","arch":"x64"})

+ vue-cli@2.9.6
+ pm2@3.3.1
added 550 packages from 405 contributors in 156.16s
```

##  node server

pm2 list

pm2 show server

pm2 stop server

## 版本

C:\Users\pc\testyarn>node -v
v11.10.0

C:\Users\pc\testyarn>npm -v
6.7.0

C:\Users\pc\testyarn>pm2 -v
3.3.1

C:\Users\pc\testyarn>yarn -v
1.15.0-20190305.1726

brew -v



## nignx服务

