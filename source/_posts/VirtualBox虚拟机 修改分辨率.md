---
title: VirtualBox虚拟机 修改分辨率
tags:
  - linux
  - VirtualBox
abbrlink: 65195
date: 2016-06-13 15:42:01
categories:
---

## 前言
  最近搞一个多人视频系统,需要用到php的多进程模块,所以搞了一个linux虚拟机,装的是号称最美linux的Elementary OS,不过VirtualBox的分辨率真让人头疼,以前只用命令行也就不在乎了,这么漂亮的桌面就那么一小块就不爽了,于是开始调整分辨率.
## 具体步骤
  1. 首先安装增强功能
  ![](/image/16-6/1.png)
  2. VirtualBox会挂载一个光盘,进入linux找到光盘位置.执行sudo sh VBboxLinuxAdditions.run 即可
  ![](/image/16-6/2.png)
  3. 调整分辨率
  ![](/image/16-6/3.png)
  
详细可看下面文章.
## 参考文章
[VirtualBox虚拟机 Ubuntu分辨率太小的解决方案](http://blog.csdn.net/yasi_xi/article/details/42388119)