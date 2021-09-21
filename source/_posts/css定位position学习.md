---
title: css定位position学习
tags: css
abbrlink: 60238
date: 2016-11-19 23:05:46
categories:
---

## 简介

| 值       | 描述   |
| --------   | :----- |
| absolute     | 生成绝对定位的元素，相对于 static 定位以外的第一个父元素进行定位。元素的位置通过 "left", "top", "right" 以及 "bottom" 属性进行规定。 |   
| relative       |  生成相对定位的元素，相对于其正常位置进行定位。因此，"left:20" 会向元素的 LEFT 位置添加 20 像素。  |  
| fixed      |    生成绝对定位的元素，相对于浏览器窗口进行定位。元素的位置通过 "left", "top", "right" 以及 "bottom" 属性进行规定。   |  
|static|	默认值。没有定位，元素出现在正常的流中（忽略 top, bottom, left, right 或者 z-index 声明）。|
|inherit|规定应该从父元素继承 position 属性的值。|

## 实例
<iframe width="100%" height="380" src="http://code.hcharts.cn/test123/4FS0lq/share/result,html,css" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

## 额外

>z-index在实际使用中,也经常遇到.虽然这个理解上没有问题,不过在实际情况中往往打不到想要的效果,不知道为什么.

**Z-index**
[Z-index可被用于将在一个元素放置于另一元素之后。](http://www.w3school.com.cn/tiy/t.asp?f=csse_zindex2)
**Z-index**
[上面的例子中的元素已经更改了Z-index。](http://www.w3school.com.cn/tiy/t.asp?f=csse_zindex1)
