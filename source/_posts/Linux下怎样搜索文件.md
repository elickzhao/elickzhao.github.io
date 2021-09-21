---
title: Linux下怎样搜索文件
tags: linux
abbrlink: 15145
date: 2016-04-30 21:39:59
categories:
---

>使用linux系统难免会忘记文件所在的位置，可以使用以下命令对系统中的文件进行搜索。搜索文件的命令为”find“；”locate“；”whereis“；”which“；”type“

# find
linux下最强大的搜索命令为”find“。
它的格式为 `find <指定目录> <指定条件> <指定动作>`；
比如使用find命令搜索在根目录下的所有interfaces文件所在位置，
命令格式为 `find / -name  'interfaces'`

查找当前目录下以@开头的文件或者目录，搜索深度为一级也就是只在当前目录找，不进入子目录
`find . -maxdepth 1 -name "@*" `

<!--more-->

# locate
使用locate搜索linux系统中的文件，它比find命令快。
因为它查询的是数据库(/var/lib/locatedb)，数据库包含本地所有的文件信息。
使用locate命令在根目录下搜索interfaces文件的命令为 `locate interfaces`

# whereis
使用”whereis“命令可以搜索linux系统中的所有可执行文件即二进制文件。
使用whereis命令搜索grep二进制文件的命令为 `whereis grep`。

# which
使用which命令查看系统命令是否存在，并返回系统命令所在的位置。
使用which命令查看grep命令是否存在以及存在的目录的命令为 `which grep`。

# type
使用type命令查看系统中的某个命令是否为系统自带的命令。
使用type命令查看cd命令是否为系统自带的命令；查看grep 是否为系统自带的命令。
`type cd` `type grep`