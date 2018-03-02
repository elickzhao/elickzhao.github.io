---
title: windows下如何生成 github 和 gitoschina 的 ssh 公钥 -- 更新版
date: 2016-05-01 20:54:10
tags: [Git,ssh]
categories:
---


>这个文档已经更新了三次了,所以看时要仔细阅读下.

1. 安装git，从程序目录打开 "Git Bash" 
2. 键入命令：ssh-keygen -t rsa -C "email@email.com"
  "email@email.com"是github账号
3. 提醒你输入key的名称，输入如id_rsa
4. 在C:\Documents and Settings\Administrator\下产生两个文件：id_rsa和id_rsa.pub
5. 把4中生成的密钥文件复制到C:\Documents and Settings\Administrator\.ssh\ 目 录下。
6. 用记事本打开id_rsa.pub文件，复制内容，在github.com的网站上到ssh密钥管理页面，添加新公钥，随便取个名字，内容粘贴刚

才复制的内容。
7. ^_^ OK了

需要注意步骤2中产生的密钥文件在当前用户的根目录，必须把这两个文件放到当前用户目录的“.ssh”目录下才能生效。
在windows中只能在命令行下输入创建"."开头的文件夹。命令为 mkdir .ssh
<!--more-->
-----

**以上的方法有可能存在问题**
使用 Git Bash 生成的公钥与私钥 放到github 经过测试 ` ssh -T git@github.com ` 却返回禁止访问

>第三次更新.
![](/image/17-1/1.jpg)
如图一 虽然显示github好像没有通过.
![](/image/17-1/2.jpg)
但如图二,其实是能更新github的.
这里还要注意一下当使用测试命令的时候会自动添加ip到 `known_hosts` 如果没有这个的话会报错如图三.
![](/image/17-1/3.jpg)


这次因为用Vagrant ssh 需要ssh程序所以下载了 cygwin `注意:cygwin的ssh也不是默认的 需要选择下载`  用这里的ssh生成的密钥就可以了 生成的方法跟上面是一致的 不过这个密钥的存放位置需要找一下 就是在cygwin的安装目录下的/home里面 `/home/elick/.ssh/id_rsa` 这里是cygwin默认的家目录
然后把公钥复制到github和oschina 用命令测试 `ssh -T git@git.oschina.net` 返回欢迎就ok了
[参考文档](http://jingyan.baidu.com/article/a65957f4e91ccf24e77f9b11.html)

~~但还有个问题 Git Tortoise用的是putty密钥 所以这个密钥虽然测试成功 但是不能用客户端来用 这个有空看看怎么办~~

上面那个问题已经 解决了 [解决的文章在这里](https://www.zybuluo.com/mdeditor#350408)

简单说明一下 就是 GitTortoise 会自带一个 putty 密码生成工具 用这个工具转换 这个密码 并用 Git Tortoise 另一个工具 Pageant 把密码填进去 以后提交git时 这个工具帮你自动转换putty密码了 同步的时候 GitTortoise有个选项转换密码 勾上就行了 详情还是看上面的那个文章

