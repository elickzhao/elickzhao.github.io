---
title: js一些隐式写法 !0 !!0 viod 0
tags:
  - js
  - 小百科
categories: 前端技术
abbrlink: 9736
date: 2016-07-01 23:28:45
---

## 前言
最近看别人的框架遇到一些别人的写法,虽然很直白,但是以前没遇到过,还是有点懵.
不知道这种写法是遵从那那些编写规则,反正记录一下已被后面查找.

## 简单说明

```js
//其实这个很直白了 因为一般bool值 表示 ture 为 1 false 为 0
!0 == true 
!!0 == false 
//但是 !0 === true 这是错的 恒等于 是不会转义类型的 所以 0 还是 int 型  所以不能与 bool 型相等

//这些都是同理了
!1 == false
!!1 == true


//这是设置 a 为 undefined , 如果用字符串代替会存在浏览器兼容问题
//也可以在 return 时使用,表示返回空,只是执行操作.
//具体看下面参考文档
var a = viod 0;

```
## 参考文档
[在js中，为什么!0是true，!!0是false，!1是false，!!1是true，!-1是false，!!-1是true](http://zhidao.baidu.com/link?url=LEMJPUPH3jrLaTXRkd299zpM2eo7zJmw3LfJxfM4wZNooN1jSU4NkOe3MyFljA8DrvkdgnApOR325XNOW2rugn8w7wIfjuCR9EN0E8kgyfS)
[JS void 0 解析](http://elickzhao.github.io/2016/06/JS%20void%200%20%E8%A7%A3%E6%9E%90/)



