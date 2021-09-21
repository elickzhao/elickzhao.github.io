---
title: js中let和var定义变量的区别 const 说明
tags:
  - js
  - 小百科
categories: 前端技术
abbrlink: 49385
date: 2016-10-27 22:33:41
---


**let** 和 **var** 是一样的用于定义,最主要的是 **let** 是javascript 严格模式.

**主要区别如下:**

- 作用范围不一样, **var**是全局的, **let**是局部的.
- 而且使用严格模式,文件开头一定要声明 **'use strict';** 否则会报错.
- 还有必须先声明再使用.
- 在同一作用域,重复声明会报错.

[参考文档](http://blog.csdn.net/nfer_zhuang/article/details/48781671)
[深入浅出ES6（十四）：let和const](http://www.infoq.com/cn/articles/es6-in-depth-let-and-const/)
[在JavaScript ES6中使用let和const定义变量](http://www.tuicool.com/articles/zEruqm)


