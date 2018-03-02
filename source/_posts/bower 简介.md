---
title: bower 简介
date: 2016-05-18 16:00:08
tags: [bower,前端]
categories: 前端技术
---

详细内容 请参考这篇文章
[Bower的简单使用教程](http://my.oschina.net/Nealyang/blog/632930?fromerr=3goAA2bF)

-------

`bower init` 生成 bower.json 这个是项目安装的组件包
`bower --save XXX ` 这是本项目安装组件 并记录在bower.json

bowerrc文件简单说明

>directory:是值第三方依赖包下载所在的位置
proxy：在很多公司，为了保护公司内网的安全性，是需要配置这个代理的。所以这里一般要配置公司的代理
timeout：默认的连接为60000ms，但是当网速不好的时候，可以设置10分钟，”timeout”:600000
还有很多大家可以自行查一查哈