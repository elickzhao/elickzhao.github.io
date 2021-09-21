---
title: 解决ln -s 软链接产生Too many levels of symbolic links错误
tags:
  - linux
  - 服务器相关技术
categories: 服务器端
abbrlink: 24332
date: 2016-04-18 11:50:14
---

今天生成软连接发生这个报错,查了一下,原来是因为使用了相对路径,改成绝对路径就没有问题了
```bash
 $ln -s /cygdrive/f/Vagrant /home/elick/vagrant
```

>命令解析:
ln [参数][源文件或目录][目标文件或目录]