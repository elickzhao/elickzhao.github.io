---
title:  JSFiddle 改写展示标签
date: 2016-07-30 21:34:19
tags: [小百科,前端]
categories: 前端技术
---

## 前言
[JSFiddle](https://jsfiddle.net/elick/s03Lk51x/1/) 是一个用于前端代码展示与分享的网站,可以在这里写代码并测试,同时写完后可以用连接分享在博客里展示给大家.在国内也有个类似网站 [HCODE](http://code.hcharts.cn/blog-demo/OYZT3i/1)

## 简单讲解
  原有的标签顺序狠不好,JS文件在前显示结果在后.所以要改一下,把结果放在前面.
  `//jsfiddle.net/elick/s03Lk51x/embedded/result,js,html,css`
  其实很简单,`embedded/`后面这个结构就是标签顺序.

## 参考文档
[CnBlogs博文demo演示技巧比较：jsfiddle完胜](http://www.cnblogs.com/iamzhanglei/archive/2011/10/07/2199306.html)
[类似jsfiddle的网站还有哪些？](https://segmentfault.com/q/1010000000339531)
