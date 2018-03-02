---
title: composer 一些常用命令及使用心得
date: 2016-05-06 17:30:51
tags: [php,composer]
categories:
---

```
composer selfupdate                      
composer require "foo/bar:1.0.0"                    安装一个库
composer update foo/bar                             更新单个库
composer create-project laravel/laravel myapp --prefer-dist 创建laravel项目
composer config -g repo.packagist composer https://packagist.phpcomposer.com 配置仓库镜像
composer global require "laravel/installer=~1.1"    全局安装laravel安装器

composer install update --prefer-dist               后面这个参数是强制使用压缩包
composer install --profile                          后面这个参数是显示安装时间
composer dump-autoload --optimize                   生产环境优化
composer update symfony/yaml --prefer-source        强制克隆代码 用于修改库文件

当你更新一个修改的库的时候 会提示你是否放弃修改
----------------------------------------------------------------------------------------------------
$ composer update
Loading composer repositories with package information  
Updating dependencies  
  - Updating symfony/symfony v2.2.0 (v2.2.0- => v2.2.0)
    The package has modified files:
    M Dumper.php
    Discard changes [y,n,v,s,?]?
----------------------------------------------------------------------------------------------------

```
**全局配置目录**
C:\Users\elick\AppData\Roaming\Composer
