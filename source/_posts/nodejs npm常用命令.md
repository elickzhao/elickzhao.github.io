---
title: nodejs npm常用命令
tags:
  - npm
  - node.js
categories: 前端技术
abbrlink: 41231
date: 2016-05-06 22:47:16
---

**npm search <name>** 搜索包

**npm install <name>** 安装nodejs的依赖包

**npm install <name> -g**  将包安装到全局环境中
但是代码中，直接通过require()的方式是没有办法调用全局安装的包的。全局的安装是供命令行使用的，就好像全局安装了vmarket后，就可以在命令行中直接运行vm命令

**npm install <name> --save**  安装的同时，将信息写入package.json中

项目路径中如果有package.json文件时，直接使用npm install方法就可以根据dependencies配置安装所有的依赖包
这样代码提交到github时，就不用提交node_modules这个文件夹了。

**npm init**  会引导你创建一个package.json文件，包括名称、版本、作者这些信息等

**npm ls**  列出当前安装的了所有包

**npm ls -g** 查看全局安装的模块及依赖

<!--more-->

**npm root** 查看当前包的安装路径

**npm root -g**  查看全局的包的安装路径

**npm config set cache** 设置全局缓存

**npm config set prefix**  设置全局安装包位置

**npm update <name>** 更新

**npm remove <name>** 移除

**npm uninstall xxx  (-g)** 卸载模块

**npm cache clean** 清理缓存

**npm help**  帮助，如果要单独查看install命令的帮助，可以使用的npm help install

**npm view moudleName dependencies**：查看包的依赖关系

**npm view moduleName repository.url**：查看包的源文件地址

**npm view moduleName engines**：查看包所依赖的Node的版本

**npm help folders**：查看npm使用的所有文件夹

**npm rebuild moduleName**：用于更改包内容后进行重建

**npm outdated moduleName**：检查包是否已经过时，此命令会列出所有已经过时的包，可以及时进行包的更新

参考文章:
[http://www.cnblogs.com/linjiqin/p/3765772.html](http://www.cnblogs.com/linjiqin/p/3765772.html)
[http://my.oschina.net/robinjiang/blog/168732](http://my.oschina.net/robinjiang/blog/168732)