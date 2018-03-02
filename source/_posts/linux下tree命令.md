---
title: linux下tree命令
date: 2016-05-27 12:47:57
tags: [小百科,linux]
categories: 服务器端
---

## 前言
windows下是有`tree`命令的,可以打印出目录结构.但是linux下却不是默认安装的.
>Note:准确的说是ubuntu下没有,因为其他发行版本我还没有试过.

## 首先安装 **tree**

```shell
sudo apt-get install tree
```

## 简单使用

```shell
# 星号代表层级数,想看到那层写那个,默认全都显示.
# 而且不想windows只显示目录,默认是会显示目录和文件
tree -L * 

# 只显示目录 不显示文件
tree -d 

# 更多说明看这个吧 都很简单
tree --help
```

##文章参考:
[linux tree命令以树形结构显示文件目录结构](http://jingyan.baidu.com/article/acf728fd19c7eff8e510a3eb.html)
