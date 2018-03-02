---
title: laravel-breadcrumbs 面包屑插件
date: 2016-06-10 15:44:07
tags: [laravel,php]
categories: php开发
---

## 前言
    还不错的插件,减少了开发面包屑功能的麻烦,不过还得自定义个文件,每个route都得写上下关系才行.还是不能特别便利,不过有总比没有好.
    
## 使用方法
- 安装
```
$ composer require davejamesmiller/laravel-breadcrumbs
```
<!--more-->

- 配置
```php
'providers' => [
    // ...
    DaveJamesMiller\Breadcrumbs\ServiceProvider::class,
],

'aliases' => [
    // ...
    'Breadcrumbs' => DaveJamesMiller\Breadcrumbs\Facade::class,
],
```

- 使用

需要创建个文件`app/Http/breadcrumbs.php` 像是这样.每个路由都得定义一下,并且给与上下级关系,
下面只是静态定义,更多定义可以看文档.
```php
<?php

// Home
Breadcrumbs::register('home', function($breadcrumbs)
{
    $breadcrumbs->push('Home', route('home'));
});

// Home > About
Breadcrumbs::register('about', function($breadcrumbs)
{
    $breadcrumbs->parent('home');
    $breadcrumbs->push('About', route('about'));
});

// Home > Blog
Breadcrumbs::register('blog', function($breadcrumbs)
{
    $breadcrumbs->parent('home');
    $breadcrumbs->push('Blog', route('blog'));
});

// Home > Blog > [Category]
Breadcrumbs::register('category', function($breadcrumbs, $category)
{
    $breadcrumbs->parent('blog');
    $breadcrumbs->push($category->title, route('category', $category->id));
});

// Home > Blog > [Category] > [Page]
Breadcrumbs::register('page', function($breadcrumbs, $page)
{
    $breadcrumbs->parent('category', $page->category);
    $breadcrumbs->push($page->title, route('page', $page->id));
});
```

路由也需要一定的操作,这里文档没有特别提出.这里需要定义路由时要命名路由要不然就会出错.
```php
//利用as定义路由别名
Route::get('home',['as'=>'home',function(){
    return view('home');
}]);

Route::get('blog',['as'=>'blog',function(){
    return view('blog');
}]);
```

在模版中使用
```
$ php artisan vendor:publish
//生成 config/breadcrumbs.php 配置文件
```
这里可以配置模版文件,也就是面包屑样子.可以自定义,也可以使用插件写好的样子.文件像这样.
```php
'view' => 'breadcrumbs::bootstrap3',
```

然后在模版文件添加代码
```php
<link href="http://cdn.bootcss.com/bootstrap/3.3.6/css/bootstrap.min.css" rel="stylesheet">
<script src="http://cdn.bootcss.com/jquery/2.2.3/jquery.min.js"></script>
<script src="http://cdn.bootcss.com/bootstrap/3.3.6/js/bootstrap.min.js"></script>
{!! Breadcrumbs::render('blog') !!}
```
自带模版是拿bootstrap写的,所以要加入bootstrap样式,要不该看不到样子了

好了,简单使用就这些了,更多用法看文档吧,文档很详细的.

## 参考文档

**仓库地址**
[davejamesmiller/laravel-breadcrumbs](https://github.com/davejamesmiller/laravel-breadcrumbs/blob/master/docs/start.rst)

**文档地址**
[Laravel Breadcrumbs](http://laravel-breadcrumbs.davejamesmiller.com/en/latest/start.html)