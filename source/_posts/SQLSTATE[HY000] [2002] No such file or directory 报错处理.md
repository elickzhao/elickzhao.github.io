---
title: 'SQLSTATE[HY000] [2002] No such file or directory 报错处理'
tags:
  - php
  - laravel
  - 小百科
abbrlink: 59222
date: 2016-11-11 23:01:49
categories:
---

## 简介
在装lumen时突然出现了这个错误,主要是因为多个版本php共存,新装的php7配置php.ini时,没有把`*.default_socket`设置上而造成无法连接数据库的问题.

## 解决

- 首先说明这个是在使用nginx时,必须使用php-fpm时出现的.
- 现在找到 `socket` 的位置. 使用命令 `ps aux | grep -i mysql` 看到如下结果
>mysql    12388  0.0  0.2 390744 44636 pts/0    Sl   14:02   0:01 /opt/mysql/product/bin/mysqld --basedir=/opt/mysql/product --datadir=/opt/mysql/product/data --plugin-dir=/opt/mysql/product/lib/plugin --user=mysql --log-error=/opt/mysql/product/data/oracle1.err --pid-file=/opt/mysql/product/data/oracle1.pid --**socket**=/var/lib/mysql/mysqld.sock --port=3306  `这就是位置`

- 然后打开 `php.ini` 添加 `/var/lib/mysql/mysqld.sock` 默认为空
 ```
// mysql.default_socket =  // php7里已经没有了这个
pdo_mysql.default_socket=/var/lib/mysql/mysqld.sock // 默认是空的,这个就是新添加的地址
mysqli.default_socket =/var/lib/mysql/mysqld.sock
```
- 然后重启 `php-fpm` 就解决了.

## 扩展阅读
- php7 以后就不实用`mysql`这个驱动了 而使用`mysqlnd` 所以你在`phpinfo()`里不会再看到mysql项
- 重启`php-fpm` 需要用 `ps aux|grep php` 命令查看 `php-fpm` 的 pid,然后 `kill pid`.这里注意啊 要是多个`php-fpm`一定要看配置文件位置,不要删错了.

## 参考文档
[YII2数据库操作出现类似Database Exception](http://www.th7.cn/db/mysql/201412/83563.shtml)
[安装roundcube 时出现 “DSN (write): NOT OK(SQLSTATE[HY000] [2002] No such file or directory)”](http://www.cnblogs.com/AloneSword/p/3188012.html)
[php7不支持mysql扩展，如何使用mysqlnd?](https://segmentfault.com/q/1010000004241965)
