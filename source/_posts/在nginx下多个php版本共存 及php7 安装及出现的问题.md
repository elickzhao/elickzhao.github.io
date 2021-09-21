---
title: 在nginx下多个php版本共存 及php7 安装及出现的问题
tags:
  - php
  - 服务器相关技术
  - 小百科
abbrlink: 4986
date: 2016-11-15 23:19:32
categories:
---



在nginx下多个php版本的时候,需要启动两个php-fpm,在配置时php7把当前服务的配置放在了`usr/local/etc/php-fpm.d/`下了,默认里面有个default,不过需要改成 `www.conf` 否则就会出现下面那个错误.

>3.WARNING: Nothing matches the include pattern '/usr/local/etc/php-fpm.d/*.conf' from /usr/local/etc/php-fpm.conf at line 125.
ERROR:. No pool defined at least one pool section must be specified in config file
ERROR: failed to post process the configuration
ERROR: FPM initialization failed

>solution:

>cp www.conf.default www.conf


php.ini需要配置
```
#这里的.sock位置 需要 ps aux | grep -i mysql 命令查询
pdo_mysql.default_socket=/var/lib/mysql/mysqld.sock
mysql.default_socket =/var/lib/mysql/mysqld.sock
mysqli.default_socket =/var/lib/mysql/mysqld.sock
```
否则出现下面报错
`SQLSTATE[HY000] [2002] No such file or directory`


安装php7 主要看下面的 `Linux环境PHP7.0安装`

编译时出现的问题,主要会报错一些插件没有安装,主要看下面的 `编译安装PHP 时遇到问题解决方法.`
这里每次出现的报错不会相同,主要看你的linux发行版环境而定.

[Linux环境Nginx安装多版本PHP](http://blog.csdn.net/21aspnet/article/details/47658127)
[编译安装PHP 时遇到问题解决方法.](http://www.cnblogs.com/z-ping/archive/2012/06/18/2553929.html)
[解决error: xslt-config not found. Please reinstall the libxslt >= 1.1.0 distribution](http://www.admpub.com/blog/?post=169)
[Linux环境PHP7.0安装](http://blog.csdn.net/21aspnet/article/details/47708763)
