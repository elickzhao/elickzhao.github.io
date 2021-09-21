---
title: 在VirtualBox和Vagrant中安装Docker
comments: true
categories: 服务器端
tags:
  - linux
  - 服务器相关技术
  - vagrant
abbrlink: 34978
date: 2016-04-14 01:18:48
---

# 参考文章
[在VirtualBox和Vagrant中安装Docker](http://www.jdon.com/artichect/docker-install.html)
[OSX下使用vagrant和docker管理创建虚拟环境](http://www.tuicool.com/articles/V36R3y)
[Vagrant简介和安装配置](http://rmingwang.com/vagrant-commands-and-config.html)
[使用 Vagrant 搭建本地开发环境的教程](http://ninghao.net/blog/1566)
[使用 Vagrant 和 Docker 在一个 VM 中进行开发](http://www.oschina.net/translate/unsuck-your-vagrant-developing-in-one-vm-with-vagrant-and-docker)
[1+1>2:用Docker和Vagrant构建简洁高效开发环境](http://cloud.51cto.com/art/201503/470256_all.htm)
[Vagrant运行Docker的几种方法](http://blog.sina.com.cn/s/blog_72ef7bea0102vucz.html)
[vagrant在windows下的使用](http://www.cnblogs.com/ac1985482/p/4029315.html)
[服务器操作系统CoreOS初体验](http://www.blogjava.net/yongboy/archive/2013/08/26/403325.html)
['当流浪者(Vagrant)遇见码头工人(Docker)'系列](http://betacz.com/series/%E5%BD%93%E6%B5%81%E6%B5%AA%E8%80%85%28Vagrant%29%E9%81%87%E8%A7%81%E7%A0%81%E5%A4%B4%E5%B7%A5%E4%BA%BA%28Docker%29/)

<!--more-->

# 自我理解
用boot2docker启动docker 容器一直会卡在 `default: Syncing folders to the host VM...` 看了下[官方文档](https://www.vagrantup.com/docs/docker/basics.html)
>Synced folder note: Vagrant will attempt to use the "best" synced folder implementation it can. For boot2docker, this is often rsync. In this case, make sure you have rsync installed on your host machine. Vagrant will give you a human-friendly error message if it is not.

~~这尿性是要安装一个rsync的同步软件 这鸟东西安装起来蛮麻烦的 而且基本用不到 所以不装了  毕竟就算是生产环境肯定用不到这个 linux版本 所以开发的时候还得装别的版本 以达到和生产环境同步 所以算了
直接搞个 CoreOS 这个试试吧 这个挺新的比较有趣~~
最后还是测试了rsync这个软件,因为使用CoreOS加载docker镜像 也会出现卡在同步文件夹那 所以看来问题不在boot2docker 

# 得到升华
**我找到个方法 可以解决这个问题 但是并不是十全十美的方法**
就是屏蔽同步文件夹
```vb
Vagrant.configure(2) do |config|
  config.vm.synced_folder ".", "/vagrant", disabled: true  #这一句就是
  config.vm.provider "docker" do |d|
    d.image = "tknerr/baseimage-ubuntu:14.04"
    d.has_ssh = true

    #这句是新加的
    #d.vagrant_machine = "dockerhost" 
   	#d.vagrant_vagrantfile = "../coreos/Vagrantfile"
    #config.vm.box = "coreos-stable" 
  end
  config.vm.provision "shell", inline: "echo 'hello docker!'"
end
```
## 惊喜发现
**这个简直太省地方了:** 利用boot2docker 加载 tknerr/baseimage-ubuntu:14.04 这个基础镜像 最后生成的虚拟机才有400M coreOS是900M多  而Homestead 已经是2.67G了
![hello](/image/16-4/1.png)

而且使用的box也非常省 boot2docker只有20M而已

## 目录说明
**C:\Users\elick\.vagrant.d** 这是vagrant的根目录 最主要有两个目录

- boxes
   这个是存放box的目录,所有你下载的各种box都在这里 
- data
   这个是存放host的一些信息的地方
    - docker-host
        这个就是用docker,而没有加载box的时候 自动下载的boot2docker
    - machine-index
        存着建立多少个host和host状态的信息 就是那个index 如果在virtualbox删除了host 这里还显示可以在这里手动删了

**C:\Users\elick\VirtualBox VMs** 就是virtualbox存放虚拟机的位置