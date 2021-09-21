---
title: 说明 -- RESTful架构
tags: 小百科
abbrlink: 23415
date: 2016-06-29 22:19:42
categories:
---

简单来讲　就是用 http 各种动作来实现 应用分类 
书面点就是这个
>客户端通过四个HTTP动词，对服务器端资源进行操作，实现"表现层状态转化"。
具体来说，就是HTTP协议里面，四个表示操作方式的动词：GET、POST、PUT、DELETE。它们分别对应四种基本操作：GET用来获取资源，POST用来新建资源（也可以用于更新资源），PUT用来更新资源，DELETE用来删除资源。

一般情况下 URI 应为名词  但我理解为了清楚表示 其实动词也无所谓 这点没那么重要 laravel 就也使用了动词

[详解 :　http://www.ruanyifeng.com/blog/2011/09/restful](http://www.ruanyifeng.com/blog/2011/09/restful)