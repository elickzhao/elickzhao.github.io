---
title: margin-top失效的解决
date: 2016-06-30 21:47:30
tags: [css,前端,小百科]
categories: 前端技术
---

## 前言
今天遇到个很bug的问题.在chrome里当窗口很小的时候,一个div就会偏移,但是当用鼠标调正窗口,又会变好.而且只有在chrome下有这个问题,最后确认原来是margin-top失效的原因.**再次说明:** 上面那个问题,并没有因使用`overflow:hiden`而解决.只是放在火狐里这个bug不存在了.后来是通过删除了子容器的 `float:left` 解决的.但到底是哪里冲突还是不明白.但下面这些方法解决一些浮动漂移是有用的.

## 问题的解决
正统说法:
>1：“在CSS2.1中，水平的margin不会被折叠；垂直margin可能在一些盒模型中被折叠…”
2： 当第一个层浮动，而第二个没浮动层的margin会被压缩，详见--浮动元素后非浮动元素的margin的处理。

一 如果是两个容器并列,一般出现问题,是因为第一个容器加了浮动,第二个没有加 所以造成第二个margin出问题.解决办法是在第二个容器前增加一个`<divstyle="clear:both;"></div>`.像下面这样,或者给box2也增加float;

<!--more-->

```html
<div style="background-color: red;"> 
<div class="box1" style="float:left;background-color: olive">dddd</div> 
<div class="box2" style="clear:both;margin-top:20px; background-color: rosybrown;">eeee</div> 
</div> 
```
二 还有一种情况是因为父容器而导致偏移,比如我遇到这个,我最后就是加了个`overflow:hidden;`.解决办法有一下三种:
1. 给父容器box加overflow:hidden;属性
2. 父容器box加border除none以外的属性
3. 用父容器box的padding-top代替margin-top

## 参考文档
[ margin-top在chrome中的问题](http://bbs.blueidea.com/thread-2884956-1-1.html)
[CSS中margin-top属性失效问题解决](http://developer.51cto.com/art/201009/225816.htm)
[CSS 的overflow:hidden 属性详细解释](http://jingyan.baidu.com/article/d45ad148e2a7f969552b80ae.html)