---
title: laravel 引入css和js文件
date: 2016-10-13 18:52:50
tags: [laravel,php]
categories: php开发
---

## 前言
  一般情况下是不会有问题的,类似这样 `bootstrap/css/bootstrap.min.css` 或者 `/bootstrap/css/bootstrap.min.css`,当网站使用域名的时候后者不会出错,可是当使用本地测试时,是在`http://localhost/Admin/public/admin/`这样一个路径,就会出现加载位置出错.

## 说明
  解决方法有几个 (没办法只能这么写,双括号 和 hexo冲突)

```php
## .env文件加入`APP_URL`参数 在模版里读取 env('APP_URL')
{{ env('APP_URL') }}"/libs/font-awesome/4.5.0/css/font-awesome.min.css"

## 使用帮助函数 `asset()` 这个应该算是比较正统做法 但有个问题不知道在域名下是否正确,我没测试过
{{ asset('/bootstrap/css/bootstrap.min.css') }}

## 还有他原本的形式是这样的
{{ URL::asset('//netdna.bootstrapcdn.com/bootstrap/3.0.3/css/bootstrap.min.css') }}


## 还有个一个HtmlBuilder 这个需要安装 `composer require illuminate/html 5.*` 然后这么使用
{!!Html::style('css/style.css')!!}
{!!Html::script('js/script.js')!!}
## 这挺好不用自己写那么一长串了,这个5.1里我有,5.2还没有整理
```




## 参考文档

[Laravel5如何在*.blade.php引用css/js文件](http://wenda.golaravel.com/question/828)
[laravel加载js和css等资源](http://www.cnblogs.com/ziyouchutuwenwu/p/4269715.html)
[Laravel 5 中使用 HtmlBuilder 及 URL::asset() 引入站内或站外的 css 和 js 文件](http://laravelacademy.org/post/3672.html)
