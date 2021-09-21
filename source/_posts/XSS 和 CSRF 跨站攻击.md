---
title: XSS 和 CSRF 跨站攻击
tags:
  - XSS
  - CSRF
categories: php开发
abbrlink: 5737
date: 2016-05-25 23:13:42
---


**xss 编辑**
跨站脚本攻击(Cross Site Scripting)，为不和层叠样式表(Cascading Style Sheets, CSS)的缩写混淆，故将跨站脚本攻击缩写为XSS。恶意攻击者往Web页面里插入恶意Script代码，当用户浏览该页之时，嵌入其中Web里面的Script代码会被执行，从而达到恶意攻击用户的特殊目的

>主要防范:
过滤客户提交内容,不能信任客户提交内容.目前来看chrome浏览器能够自己屏蔽这个问题.IE还是不行,还得需要强壮的代码.


**CSRF**
CSRF（Cross-site request forgery跨站请求伪造，也被称为“One Click Attack”或者Session Riding，通常缩写为CSRF或者XSRF，是一种对网站的恶意利用。尽管听起来像跨站脚本（XSS），但它与XSS非常不同，并且攻击方式几乎相左。XSS利用站点内的信任用户，而CSRF则通过伪装来自受信任用户的请求来利用受信任的网站。与XSS攻击相比，CSRF攻击往往不大流行（因此对其进行防范的资源也相当稀少）和难以防范，所以被认为比XSS更具危险性。

>主要防范:
CSRF主要是伪装请求,所以只要确认不是本站请求,就可以很好的化解这个问题.对于每个表单提交都有个token,然后提交的地址再去验证这个token就可以了

参考文献:
[XSS跨站脚本攻击过程最简单演示](http://blog.csdn.net/smstong/article/details/43561607)