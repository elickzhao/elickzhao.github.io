---
title: Postman发送带cookie的http请求
date: 2017-03-03 21:28:49
tags: [小百科,chrome,postman]
categories:
---

Postman是chrome上一个非常好用的http客户端插件，可惜由于chrome安全的限制，发不出带cookie的请求。如果想要发送带cookie的请求，需要开启Interceptor：
![](http://img.blog.csdn.net/20160615233602816)


这个Interceptor还需要到chrome应用商店下载 Postman Interceptor 扩展程序。现在能发送带cookie的http请求。发送cookie时，在header中添加key-value，key固定为Cookie，value是cookie具体的k=v，例如：

![](http://img.blog.csdn.net/20160615233624699)

需要注意的是，发送带cookie的时候必须得开着chrome浏览器。
