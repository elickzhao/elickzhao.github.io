---
title: Windows 系统下设置Nodejs NPM全局路径
tags: npm
abbrlink: 59197
date: 2016-12-10 22:11:45
categories:
---

## 前言
我因为手动安装npm造成系统里存在两个npm,而nodejs自带npm全局包位置不是很喜欢,所以手动改一下.下面是文章里写的办法,我用命令可以改,不知道他为啥非得改文件.
### 第二次安装
这次安装也出现了下面文章说的使用命令安装不上的问题,通过命令查看配置文件 `npm config ls -l` 原来是 `C:\Program Files` 这个空格的问题.使用命令后,用户配置文件`C:\Users\elick\.npmrc`里面记录的就是`C:\Program`没有后面的目录了.所以我直接改了用户配置文件,而没想下面文章写的改写全局配置.
我安装的路径`C:\Program Files\nodejs` 这个路径里`node_modules`就是放npm的默认位置,而且全局的命令也在这个目录下,所以更新npm就直接替换了.所以想来这个应该就是默认配置目录


## 简单说明

查看配置命令
`npm config ls -l`

Windows下的Nodejs npm路径是appdata，很不爽，想改回来，但是在cmd下执行以下命令也无效

npm config set cache "C:\Program Files\nodejs\node_cache"

npm config set prefix "C:\Program Files\nodejs"

最后在nodejs的安装目录中找到node_modules\npm\.npmrc文件

修改如下即可：

prefix = D:\nodejs\node_global
cache = D:\nodejs\node_global

## 参考文档
[Windows 系统下设置Nodejs NPM全局路径](http://www.cnblogs.com/picaso/p/3848209.html)
