---
title: larave后台日志插件
date: 2016-06-12 21:59:46
tags: [laravel,php]
categories: php开发
---


## 前言

也是发现了两个插件,`rap2hpoutre/laravel-log-viewer`和`ARCANEDEV/LogViewer`.第一个简单些,界面显示也简单些.第二个丰富些,不过第二个测试时不好使,可能是因为我装了两个有点冲突吧.

<!--more-->

### rap2hpoutre/laravel-log-viewer 安装使用

**安装**
```
composer require rap2hpoutre/laravel-log-viewer
```
**配置**
```php
Rap2hpoutre\LaravelLogViewer\LaravelLogViewerServiceProvider::class,
```
**添加路由**
```php
Route::get('logs', '\Rap2hpoutre\LaravelLogViewer\LogViewerController@index');
```
然后看 http://myapp/logs 就可以了

仓库地址:
[rap2hpoutre/laravel-log-viewer](https://github.com/rap2hpoutre/laravel-log-viewer)


----

### ARCANEDEV/LogViewer 安装使用
**安装**
```
composer require arcanedev/log-viewer
```

**配置**
```php
Arcanedev\LogViewer\LogViewerServiceProvider::class,
```

```
php artisan log-viewer:publish
```

**使用**
可用这个命令检查是否安装成功
```php
php artisan log-viewer:check
```
然后看 http://myapp/log-viewer 就可以了 这个不需要上面那个,还得配置路由直接就可用