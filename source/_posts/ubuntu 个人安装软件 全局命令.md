---
title: ubuntu 个人安装软件 全局命令
tags:
  - ubuntu
  - linux
abbrlink: 22021
date: 2016-06-25 21:58:09
categories:
---

## 前言
  自己装了一个sublime text ,但是不能用命令行打开,所以才想起如何做.
  
## 简单说明
  因为linux发行版本关系,每个版本软件安装位置都不是固定的,这点比windows差了点.想找到apt-get安装真的有点费劲,这个我还没找到.但是启动命令我找到了,在ubuntu下启动命令安装在 `/usr/bin` 下面. 例如我想用的sublime text,他就是 `/usr/bin/subl`  然后在把这个 拷贝到 `/usr/sbin` 下就可以是全局命令了.
`cp /usr/bin/subl /usr/sbin`  
