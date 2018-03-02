---
title: js中let和var定义变量的区别
date: 2016-10-15 21:02:46
tags: [JavaScript,小百科]
categories:
---

**let** 和 **var** 是一样的用于定义,最主要的是 **let** 是javascript 严格模式.

**主要区别如下:**

- 作用范围不一样, **var**是全局的, **let**是局部的.
- 而且使用严格模式,文件开头一定要声明 **'use strict';** 否则会报错.
- 还有必须先声明再使用.
- 在同一作用域,重复声明会报错.

[参考文档](http://blog.csdn.net/nfer_zhuang/article/details/48781671)
