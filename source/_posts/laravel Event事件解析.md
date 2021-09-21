---
title: laravel Event事件解析
tags:
  - laravel
  - php
categories: php开发
abbrlink: 59652
date: 2016-06-14 22:43:47
---

## 前言
  Event事件总是晕晕的,反复看了几遍,终于纠正了我的惯性思维,我一直认为监听类是起到监听作用.其实错了,事件类是相当于触发,监听某个动作.而监听类是响应这个动作的具体操作.从另一个方面讲,监听类其实监听的是事件类的触发.这下就清晰点了.下面把整个事件所需要的文件都简单说明下.

## 简要说明

**laravel事件的主要文件**
1. Event.php (创建事件类,这个文件最主要的作用就是注入,其他的功能暂时没发现)
2. Listener.php (创建监听类,这个文件的主要作用就是响应事件类,当事件触发了进行响应操作)
3. EventServiceProvider.php (关联事件与监听的文件)

<!--more-->

![](/image/16-6/4.svg)

**编写事件流程**
1. 注册事件-监听器 (也就是在EventServiceProvider里关联相关事件类和监听类)
2. 用php artisan event:generate 生成事件类文件和监听类文件
3. 定义事件类
4. 定义监听器类
5. 触发事件
详细看下面的文档

## 参考文档
[Laravel 5.1 定义事件、事件监听器以及触发事件实例教程](http://laravelacademy.org/post/1889.html)
