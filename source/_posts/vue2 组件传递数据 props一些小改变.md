---
title: vue2 组件传递数据 props一些小改变
tags:
  - js
  - vue
abbrlink: 13026
date: 2016-12-17 22:15:52
categories:
---


vue 和 vue2 在 props 使用上面有些不同,在 vue2 里,模版传递过去的值,必须在 html 标签里,否则不显示.
而vue使用时没有这个限制.还有就是 `<child message="hello!"></child>` 必须用双引号,单引号可能会出现问题.
再有就是 这个表示的是传递一个字符串,如果要传vue内data数据,必须绑定 `<pic-list v-bind:items="items"></pic-list>`,这样才可以传递其他类型的数据.

**例子**

<iframe width="100%" height="380" src="http://code.hcharts.cn/test123/FNE0Wo/share/result,js,html" allowfullscreen="allowfullscreen" frameborder="0"></iframe>