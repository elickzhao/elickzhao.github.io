---
title: JS void 0 解析
tags:
  - js
  - 前端
  - 小百科
categories: 前端技术
abbrlink: 18183
date: 2016-06-23 22:39:50
---

## 前言
  在修改kod时遇到这个,不太了解作用于是查了下资料,详细看这里 [JS魔法堂：从void 0 === undefined说起](http://www.cnblogs.com/fsjohnhuang/p/4146506.html)
  
## 简单说下
  在js函数里 `return void 0` 表示此函数无返回. 
  还有就是 undefined在JavaScript中并不属于保留字/关键字，因此在IE5.5~8中我们可以将其当作变量那样对其赋值（IE9+及其他现代浏览器中赋值给undefined将无效）
  所以说用 void 赋值更为正规一些.
  
  void的行为特点为：

　　1. 不管void后的运算数是什么，只管返回纯正的undefined；

　　2. void会对其后的运算数作取值操作，因此若属性有个getter函数，那么就会调用getter函数（因此会产生副作用）

  更多具体内容看上面文章吧.
　　