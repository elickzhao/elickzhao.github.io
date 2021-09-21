---
title: gulp使用bower下组件
tags:
  - gulp
  - bower
categories: 前端技术
abbrlink: 63195
date: 2016-05-11 00:59:14
---

我的项目使用 yeoman 生成的目录 自带gulpfile.js

想引入bower里组件到html页面 不需要手动写 

执行 gulp 任务 wiredep 就可以了

但后来我引用兼容插件时 没有使用 也自动写入文件了 难道是serve任务的功劳??

这个尚不清楚,反正如果没引入 执行一下这个 wiredep任务就可以了

gulp+bower 使用必须用 wiredep

手动wiredep才可以