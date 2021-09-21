---
title: PHP 扩展模块 phpize 解释
tags:
  - php
  - 小百科
abbrlink: 56095
date: 2016-06-19 21:48:04
categories:
---
## 简介
  phpize是什么？
phpize是用来扩展php扩展模块的，通过phpize可以建立php的外挂模块
比如你想在原来编译好的php中加入memcached或者ImageMagick等扩展模块，可以使用phpizen
  安装PHP的模块一个方式是加上相关参数重新编译PHP一个是用到phpize,比如eaccelerator，memcache等，这个比较方便，不用重新编译PHP，也可以随时启用或停用
  phpize是用来扩展php扩展模块的，通过phpize可以建立php的外挂模块.

<!--more-->

## 使用实例

Linux下利用phpize安装php扩展

php有很多扩展功能，我们在初次安装的时候并没有安装某些扩展，可能在使用的过程中，又需要用到这些扩展。php提供了一个phpize工具供我们安装需要的扩展。 

下面我通过安装socket扩展来介绍phpize的使用： 

1.找到自己的php安装目录，例如我的目录是`home/vsrank/php`，在该目录下，找到`bin/phpize`。如果没有这个工具，则说明没有安装该工具，那么需要安装php.dev，一般都会有这个工具。 

2.要扩展的话，就需要有一个和当前已安装的php的版本一样的php的源包，当前php版本可以用过phpinfo()查看。就是初次安装后查看安装是否成功的那个test.php。 

3.打开源包目录，进入到ext目录，例如我就进入到：`/home/vsrank/php-5.3.10/ext`下，ext下有各个php带有的扩展模块，进入到`ext/sockets`中。 

4.cd到ext/sockets后，执行下面的命令：
```
/home/vsrank/php/bin/phpize
```
即执行phpize工具，执行后，可以看到目录下生成了对应的configure文件： 

5.现在就可以通过configure来配置，执行下面的命令： 
```
./configure --enable-sockets --with-php-config=/home/vsrank/php/bin/php-config  
```
``` 
make  
make install
```
执行之后，可以看到下面的输出： 
```
Installing shared extensions:     /home/vsrank/php/lib/php/extensions/no-debug-non-zts-20090626/  
Installing header files:          /home/vsrank/php/include/php/
```
第一个就是扩展模块的生成目录，可以在对应目录下看到对应的sockets.so文件。 

6.更改php.ini，增加下面的语句： 
```
extension="/home/vsrank/php/lib/php/extensions/no-debug-non-zts-20090626/sockets.so"
```
可以看到和上面的输出是一致的。 

7.重启Apache，接下来就可以看看自己的socket是不是配置好了。。

## 参考文档
[phpize增加php模块](http://blog.51yip.com/php/177.html)
[phpize的深入理解](http://www.jb51.net/article/37741.htm)
[php教程之phpize使用方法](http://www.jb51.net/article/46706.htm)
[linux下用phpize给PHP动态添加扩展](http://my.oschina.net/junn/blog/158684)



