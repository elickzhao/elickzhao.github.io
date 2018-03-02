---
title: Nginxconflicting server name  0.0.0.080, ignored
date: 2016-11-29 23:13:56
tags: [小百科,服务器相关技术]
categories:
---



Nginx:conflicting server name * 0.0.0.0:80, ignored
在编写了nginx配置文件后，重启nginx时出现如下警告:
```
[jh@VM_84_179_centos conf.d]$ sudo /etc/init.d/nginx restart
nginx: [warn] conflicting server name "blog.jianghang.name" on 0.0.0.0:80, ignored
Stopping nginx:                                            [  OK  ]
Starting nginx: nginx: [warn] conflicting server name "blog.jianghang.name" on 0.0.0.0:80, ignored
                                                           [  OK  ]
```
这一般是由于技术员的粗心造成的。原因是blog.jianghang.name这个域名出现了两次甚至多次，把同一个域名解析到了不同的目录。一般将配置文件单独分离出来容易出现这个错误。
接下来就是解决办法：
1.查询哪些文件里面包含了 blog.jianghang.name字段 。使用grep命令，该命令实例如下：
```
sudo grep -r blog.jianghang.name /etc/nginx/conf.d/
```
参数 -r 是指明确要求搜索子目录，此处的目录就是/etc/nginx/conf.d/ ，因为我知道配置文件都是在这个目录下，所以用了 -r 参数。如果不知道具体目录，可以用 * 代替，其实就是正则表达式啦。这里不细讲。
找到了出现 blog.jianghang.name 的文件之后，就将设置不正确的地方该正确吧。

记住一个域名只能有一个对应的目录，一个目录可以有多个对应的域名。
