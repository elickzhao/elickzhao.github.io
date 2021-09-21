---
title: npm 及 node.js 升级自己
tags:
  - npm
  - node.js
  - 小百科
categories: 前端技术
abbrlink: 38400
date: 2017-01-12 12:38:53
---

# npm 自身升级
```
npm -g install npm
```

# node.js 自身升级
```
# node有一个模块叫n（这名字可够短的。。。），是专门用来管理node.js的版本的。
# 首先安装n模块：
npm install -g n 

#升级node.js到最新稳定版
n stable

# n 后面也可以跟随版本号比如： 
n v0.10.26 
```

----


有一次升级遇到问题,根本无法通过上面方法实现升级,而且会报错提示级别太低.
这是唯一的办法是下载新的windows版本重载才行. 
还有就是 `n`这个程序好像只是在linux下才好使,windows下是没用的.