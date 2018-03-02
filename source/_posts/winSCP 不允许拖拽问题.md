---
title:  winSCP 不允许拖拽问题
date: 2016-11-03 12:30:08
tags: 小百科
categories:
---

这个问题是因为windows上没有Shell扩展.解决办法是通过临时文件,再复制到目标文件夹内.

在winSCP里的 `选项->选项` 打开选项界面. 找到 `拖拽` 这个选项菜单,进行修改就ok了.

![](/image/16-11/1.png)
