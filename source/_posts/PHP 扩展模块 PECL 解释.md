---
title:  PHP 扩展模块 PECL 解释
date: 2016-06-17 11:05:36
tags: [php,小百科]
categories: php开发
---
## 简介

PECL 的全称是 The PHP Extension Community Library ，是一个开放的并通过 PEAR(PHP Extension and Application Repository，PHP 扩展和应用仓库)打包格式来打包安装的 PHP 扩展库仓库。通过 PEAR 的 Package Manager 的安装管理方式，可以对 PECL 模块进行下载和安装。与以往的多数 PEAR 包不同的是，PECL 扩展包含的是可以编译进 PHP Core 的 C 语言代码，因此可以将 PECL 扩展库编译成为可动态加载的 .so 共享库，或者采用静态编译方式与 PHP 源代码编译为一体的方法进行扩展。PECL 扩展库包含了对于 XML 解析，数据库访问，邮件解析，嵌入式的 Perl 以及 Pthyon 脚本解释器等诸多的 PHP 扩展模块，因此从某种意义上来说，在运行效率上 PECL 要高于以往诸多的 PEAR 扩展库

## 参考文档

[PECL （PHP 扩展模块） 百度百科](http://baike.baidu.com/link?url=lISvzx5m1oH0AyzFBM-QQ145DmViNDIKVA_cTzM1pzJL91cuXcis9_uTuqeQmbIFwD4tSN61KpPgMYNfwqFA_ZlUTjWady8BMDDJa2Loreu)

[PECL 扩展库安装 官方文档](http://php.net/manual/zh/install.pecl.php)

[PECL项目主页](http://pecl.php.net/)
