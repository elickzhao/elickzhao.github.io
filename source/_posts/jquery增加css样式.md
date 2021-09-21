---
title: jquery增加css样式
tags: js
categories: 前端技术
abbrlink: 39731
date: 2016-06-28 16:38:07
---

## 前言
今天脑袋陷入了打结中,jquery改变样式.遇到一个什么情况,css是个元素样式,不是针对具体的某个类,并且后添加的样式,通过jquery无法找到,所以想改这个样式特别麻烦.

## 解决与思考
  jquery只能选定出现在DOM上的元素来更改他的css样式.为什么这么说呢,css的写法是这样的,一般先是定义一个根类,然后在子类再去分别定义特殊属性.如果这样的话,jquery能选择操作子类的样式,却无法操作根类.这种情况很诡异,应该也不常见吧.例如下面
```html
<!-- 
我遇到的就是这个问题, jquery能操作pre_id这个具体的pre的css,却无法操作上面那个css,
但是这个css,还是影响了后面动态生成的pre,而且jquery没法获得后添加的这个pre也是麻烦事,
要不也可以改变后添加这个pre的也能解决
-->
<style>
.edit_body .tabs pre{
display:none;
}

#pre_id{
display:block;
}
</style>

```
**最后的解决办法:**

```js
//利用jquery添加一段样式到html,然后去覆盖根样式.
$('<style>').html('.edit_body .tabs pre{display:block;}').appendTo($('.edit_body .tabs pre'));
```



## 参考文档
[jquery如何创建style](http://zhidao.baidu.com/link?url=jrf6HyGEdwYXPB9v7JIZBMdzwh6bEPkwSeeR1A33JN5rsgQ_2fltLx0getjDDFfEfjHu9dnwz6enBcDXkFKYja)

