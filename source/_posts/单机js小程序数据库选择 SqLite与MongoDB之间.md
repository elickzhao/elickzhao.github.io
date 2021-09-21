---
title: 单机js小程序数据库选择 SqLite与MongoDB之间
tags:
  - js
  - node.js
  - MongoDB
  - sqlite
abbrlink: 5307
date: 2017-02-08 21:21:07
categories:
---

打算用vue开发个单机小程序,于是这个数据库选择就是个问题了.
看到这个小程序 [notepad](https://github.com/lin-xin/notepad) 他是用的 `localStorage` 不过这个存储有大小限制 不太适合我的要求.
于是,就看到了SqLite与MongoDB, 这两个各有优点,确实不好选.

**MongoDB**: 这个与Node.js联系紧密,速度很快,不过据说特别占空间,而且好像是运行在内存中,如果要保存在硬盘还需要特别操作,没用过所以具体如何不清楚.

**SqLite**: 小巧精悍,据说一般用于手机程序和小程序,但是他是单线程用于硬盘,所以速度比不上MongoDB,但是占用空间小,而且可以使用sql语句,所以用起来比较简单.


经过考虑,原本打算用SqLite因为这个比较符合我的要求,但是因为从来没接触过MongoDB,所以这次就用MongoDB尝试下.不好用的情况下,就换回SqLite.
