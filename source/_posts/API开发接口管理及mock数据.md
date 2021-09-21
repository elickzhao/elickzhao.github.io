---
title: API开发接口管理及mock数据
tags:
  - 前端
  - js
  - vue
  - php
abbrlink: 33826
date: 2017-08-27 23:21:41
categories:
---

这里总结了一些开发前后端分离,对于开发API的管理.不仅可以在没有后端搭建成的时候mock数据.
而且可以管理接口及版本,并生成文档.这为开发及后续整理提供很大的方便.


|网站|具有的特色|
|--|--|
|[DOClever](http://doclever.cn/project/project.html)|这个平台有很好的介绍用的都是视频真是太贴心了[教程](http://doclever.cn/help/help.html).而且可以管理团队,项目分配,模拟数据,测试接口,版本管理.真的是功能十分丰富,这个也可以导入[Swagger](http://editor.swagger.io/),只要导出json文件 当创建项目时导入就可以了,但现在只能复制导入,导入文件总说错|
|[Easy Mock](http://www.easy-mock.com/project/5995ae24059b9c566dc5145f)|这个也很好,虽然没有那么多功能.但是对于开发是测试数据已经足够了.还可以配合[Swagger](https://www.gitbook.com/book/huangwenchao/swagger/details)来管理API接口|
|[eoApi](http://eoapi.coobar.cn/#/home/project/api)|是简化版的eolinker.也是同样的不清不楚.可能他们有自己一套API开发逻辑吧,搞不懂.有数据字典结果跟接口一毛钱关系没有.而且没有mock数据.这个都不如上面两个好用|
|[eolinker](https://www.eolinker.com/#/home/project/api)|这个也还行,自带数据库管理.可能会根据数据库来对接口进行操作吧.功能也是十分强大.但是模拟数据这一块让人摸不着头脑.还有就是本地测试需要插件,文档教程不清.上手复杂一些.还有一点是收费,以后可能功能受到限制.但是有接口市场可以简化一些操作比如连接微信什么的|
|[RAP](http://rapapi.org/org/index.do)|功能也非常强大,但是操作有点反人类,还有就是服务器不是独立的经常会慢或者崩溃.不太推荐|
