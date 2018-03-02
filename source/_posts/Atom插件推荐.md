---
title: Atom插件推荐
date: 2017-01-06 22:48:56
tags: [IDE,atom]
categories:
---

下面的文章都有具体介绍了,就不一一写了,感觉比较常用或者比较易忘的记录一下.

**sync-settings** 
插件备份、按键绑定备份
[插件主页面](https://atom.io/packages/sync-settings)
安装主页已经写了,最核心的是需要注册一个gist用于同步的仓库,这个在Github主页里能找到,gist好像是一个用于文本同步的东西.
这个是安装时的难点,使用就很简单了,把名字写上后,每次打开atom都会同步插件的.

>今天在另一个电脑上安装了,看来还是的记录下才行,没想象中那么容易

**安装**
只需要填入 Personal Access Token 和 Gist ID 就可以了  
Personal Access Token : `91c2f32691fee3e264df51417a3a8f40f7d7850b`
Gist ID  : `b65042216cbcb67891be5dbcc34c0edf`

**使用**
这里差点弄错.
如果是备份的话,就点扩展包里菜单的 `sync-settings:backup`
恢复就用扩展包里菜单的 `sync-settings:restore`

所以在新装电脑中点 `sync-settings:restore` 就能同步了,千万不要点 `sync-settings:backup`

## 参考文档
[ATOM基础教程一ATOM插件推荐(4)](http://blog.csdn.net/zsl10/article/details/51822715)
[Atom 基础教程](http://blog.csdn.net/column/details/atom-tutorial.html)
