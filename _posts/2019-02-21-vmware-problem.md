---
layout: post
title:  "VMware Workstation 与 Device/Credential Guard 不兼容问题解决!"
date:   2015-02-10 15:14:54
categories: jekyll
tags: jekyll
excerpt: vmware问题解决。
mathjax: true
---

* content
{:toc}

## VMware Workstation 与 Device/Credential Guard 不兼容报错信息：
```js
VMware Workstation 与 Device/Credential Guard 不兼容。在禁用 Device/Credential Guard 后，可以运行 VMware Workstation。有关更多详细信息，请访问 http://www.vmware.com/go/turnoff_CG_DG。
```

## 解决办法

```js

主要是win的虚拟机和vmware起冲突了
禁用服务即可：
bcdedit /set hypervisorlaunchtype off
重启win主机，即可使用。
```
