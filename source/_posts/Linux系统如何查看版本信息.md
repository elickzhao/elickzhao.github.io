---
title: Linux系统如何查看版本信息
tags:
  - 小百科
  - 服务器相关技术
  - linux
categories: 服务器端
abbrlink: 52205
date: 2016-06-06 15:41:04
---

[Linux系统如何查看版本信息](http://jingyan.baidu.com/article/7908e85c725159af481ad2f7.html)

```
//输入"uname -a ",可显示电脑以及操作系统的相关信息。
uname -a

//输入"cat /proc/version",说明正在运行的内核版本。
cat /proc/version

//输入"cat /etc/issue", 显示的是发行版本信息
cat /etc/issue

//lsb_release -a (适用于所有的linux，包括Redhat、SuSE、Debian等发行版，但是在debian下要安装lsb)
lsb_release -a 
```