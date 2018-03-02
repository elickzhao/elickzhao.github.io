---
title: git 删除右键菜单
date: 2016-05-04 22:06:10
tags: Git
categories:
---

首先，我表示git默认的右键菜单很烦，太多项了，而我们平时用的最多的无非是一个Git Bash！
删除msGit右键菜单

如果是windows 64位系统
cmd进入"C:\Program Files (x86)\Git\git-cheetah"目录，运行

```
regsvr32 /u git_shell_ext64.dll
```

如果还想用这个功能 可以看下面这个文章

参考文章 [http://blog.csdn.net/songques/article/details/8488061](http://blog.csdn.net/songques/article/details/8488061)