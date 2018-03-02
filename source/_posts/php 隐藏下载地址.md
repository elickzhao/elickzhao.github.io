---
title: php 隐藏下载地址
date: 2016-07-19 23:38:37
tags: php
categories: php开发
---

php下载时不想让别人看到真实的下载地址?怎样隐藏下载文件的真实地址?
首先要取得下载文件的URL，这里假设你通过PHP的操作取得文件的URL地址，变量为$URL
代码如下：

```php
$file_size = filesize($url); 
header("Content-type: application/octet-stream"); 
header("Accept-Ranges: bytes"); 
header("Accept-Length: $file_size");
header("Content-Disposition: attachment; filename=".basename($url)); 
header("location: $url");
```
将上面代码加入PHP文件后，就可以隐藏真实的URL地址，当用户通过点击像http://localhost/soft.php?id=1这样的网址时，就可以下载了

上面代码还有另一个功能，就是强制浏览器保存文件，而不是在浏览器当中打开文件。


## 参考文档
[php,下载时不想让别人看到真实的下载地址?怎样隐藏下载文件的真实地址](http://blog.sina.com.cn/s/blog_8edc37a80101cz9v.html)
[使用php隐藏下载文件的真实地址](http://www.cnblogs.com/xcxc/archive/2013/03/04/2943124.html)
