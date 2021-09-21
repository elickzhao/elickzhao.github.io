---
title: hexo 博客小技巧
tags: 小百科
abbrlink: 59594
date: 2016-07-31 22:32:55
categories:
---

hexo会把所有的MD文件转换成html文件.但是这样Github的`Readme`就没法展现了.我用的方法是把`Readme.md` 前面加了`_`变成`_Readme.md`,这样hexo就不会转换了,因为hexo默认过滤掉`_`开头文件或文件夹,除了`_post`.我把`Readme.md`直接扔到`public`文件夹.这样就不会转换成html文件了,这样就能是Github有Readme了.