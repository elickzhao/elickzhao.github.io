---
title: 常用的vagrant命令
tags:
  - 服务器相关技术
  - vagrant
categories:
  - 服务器端
abbrlink: 35457
date: 2016-04-20 22:28:00
---

常用的vagrant命令:
```
 $ vagrant box add NAME URL    #添加一个box
 $ vagrant box list            #查看本地已添加的box
 $ vagrant box remove NAME virtualbox #删除本地已添加的box，如若是版本1.0.x，执行$ vagrant box remove  NAME
 $ vagrant init NAME          #初始化，实质应是创建Vagrantfile文件
 $ vagrant up                   #启动虚拟机
 $ vagrant halt                 #关闭虚拟机
 $ vagrant destroy            #销毁虚拟机
 $ vagrant reload             #重启虚拟机
 $ vagrant package            #当前正在运行的VirtualBox虚拟环境打包成一个可重复使用的box
 $ vagrant ssh                 #进入虚拟环境
 
 $ vagrant init laravel/homestead #初始化并下载box
 $ vagrant box add base precise64.box #base 表示指定默认的box，也可以为box指定名称，使用base时，之后可以直接使用 vagrant init 进行初始化，如果自行指定名称，则初始化的时候需要指定box的名称。
```