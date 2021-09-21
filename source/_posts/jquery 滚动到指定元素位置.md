---
title: jquery 滚动到指定元素位置
tags:
  - js
  - 小百科
abbrlink: 24372
date: 2018-02-02 20:56:58
categories:
---

今天做表单验证,验证后想要跳到错误位置.原本打算用 **focus()** 可是用了 **layui** 根本定位不到
这也是 **layui** 一个很大的弊端.  于是想到根据元素来跳转.

```js
$('html, body').scrollTop(100);  //跳到指定位置   前面的参数也可以是id

//动画跳转
$('html, body').animate({
    scrollTop: $("#comment2").offset().top
}, 1000);

```

但是我这个用不了动画跳转,可能是因为 **iframe** 的原因,高度总是不对.所以直接用了指定位置.
而不是根据元素来跳转.


<iframe width="100%" height="450" src="http://code.hcharts.cn/blog-demo/rJFXza/share/result,js,html,css" allowfullscreen="allowfullscreen" frameborder="0"></iframe>