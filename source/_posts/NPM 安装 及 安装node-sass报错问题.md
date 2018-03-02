---
title: NPM 安装 及 安装node-sass报错问题
date: 2016-08-01 20:40:53
tags: [前端,node.js,npm]
categories: 前端技术
---


> 这次安装是按照网上教程来的 不过好像有点老了 下次尝试新的  就是不用clone npm的git 因为看到现在下载的node.js里的node_modules里 就有了这个目录了 也许直接执行命令就好了

1. 下载node.js安装 如果没有git的话下载git安装 或者直接下载git://github.com/isaacs/npm.git  压缩包也可以
2. git clone --recursive git://github.com/isaacs/npm.git 下载NPM
3. 在NPM目录里 执行安装 node cli.js install npm -gf   (-gf 为全局安装 如果安装其他包也可以使用这个命令)
4. NPM -v 查看版本 是否安装成功
 
----

## 再次安装心得

npm已经是附带安装,所以不用再像上面那样手动安装npm.
不过默认安装的npm默认安装模块位置在`C:\Users\elick\AppData\Roaming`这个下面,不像手动安装是在nodejs目录里,而且自动安装的版本也比较低.
所以选择手动安装也不错.不过必须先得写在已经安装好的npm `npm uninstall npm`

--------------

在windows下开发真是麻烦重重,还的需要安装python,还得需要安装c++.比如说要用node-sass 就得必须用这两个. 而要开发socket.io和ionic都需要这个


## 第三次安装
有时网上克隆下来的项目,还是有错误问题.又一次遇到了node-sass,这次用cnpm居然就可以了.所以可能上次安装的问题也是因为这个,有机会下次测试下.