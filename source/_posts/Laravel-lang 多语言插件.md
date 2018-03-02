---
title: Laravel-lang 多语言插件
date: 2016-06-10 21:47:26
tags: [laravel,php]
categories: php开发
---

## 前言
    发现两个不错的语音包,caouecs/Laravel-lang和overtrue/laravel-lang.第一个是第二个的基础,只是做了一下自动化

### caouecs/Laravel-lang
其实使用很简单,就如下所说,把项目下载到项目里,然后把要使用的语言包,放到`resources\lang`里,然后改写`config\app.php`里的语言设置就ok了

<!--more-->

仓库地址:
[https://github.com/caouecs/Laravel-lang](https://github.com/caouecs/Laravel-lang)

>Via Composer
For Laravel 5.* : add "caouecs/laravel-lang": "~3.0" in your composer.json in "require"
For Laravel 5 : add "caouecs/laravel4-lang": "~2.0" in your composer.json in "require"
For Laravel 4 : add "caouecs/laravel4-lang": "~1.0" in your composer.json in "require"
Do "composer update"
Files of languages are in "vendor/caouecs/laravel-lang" directory
Copy the folders of languages that you want, in app/lang (resources/lang in laravel 5) folder of your application Laravel

>Via GitHub
Clone the GitHub repository : git clone https://github.com/caouecs/Laravel-lang.git
Or download the zip file
Choose the branch:
laravel4 for Laravel4 project
master for Laravel5 project
Copy the folders of languages that you want, in app/lang folder of your application Laravel


### overtrue/laravel-lang
这个也很简单

- 安装
```
composer require "overtrue/laravel-lang:~3.0"
```

- 配置

完成上面的操作后，将项目文件 config/app.php 中的下一行
```php
Illuminate\Translation\TranslationServiceProvider::class,
```
替换为：
```php
Overtrue\LaravelLang\TranslationServiceProvider::class,
```
修改项目语言 config/app.php：
```php
'locale' => 'zh-CN',
```

- 使用
```
$ php artisan lang:publish zh-CN,zh-HK,th,tk
```

这样就会自动把语音包放到lang下面了

参考文档:
[项目说明文档](https://github.com/overtrue/laravel-lang/blob/master/README_CN.md)
[项目仓库](https://github.com/overtrue/laravel-lang)