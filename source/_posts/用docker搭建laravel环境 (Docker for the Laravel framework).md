---
title: 用docker搭建laravel环境 (Docker for the Laravel framework)
tags:
  - docker
  - laravel
  - 服务器相关技术
categories: 服务器端
abbrlink: 65384
date: 2016-04-25 23:54:28
---

# 前言
> 最近一直想搭建个自己的docker开发环境,找了不少资料.在docker-hub上发现这个,虽然内容老一点,不过思路还是很好的.而且发现用docker搭建原来是如此的so easy! 好吧,让我们开始吧.


# 首先来看看布局结构
![布局图](/image/16-4/4.png)

简单说明下.每一个服务用了一个容器.所有容器的数据都指向数据容器.这样统一管理的同时,也方便修改和报错.这里原作者把artisan和composer也放在一个容器里,但是我个人感觉这样不是太好.当然为了保持宿主主机的纯净度来讲,这是个正确的选择,就是操作起来太费事了.

<!--more-->

# 下载所有镜像

```bash
$ docker pull dylanlindgren/docker-laravel-data && \
  docker pull dylanlindgren/docker-laravel-composer && \
  docker pull dylanlindgren/docker-laravel-artisan && \
  docker pull dylanlindgren/docker-laravel-phpfpm && \
  docker pull dylanlindgren/docker-laravel-nginx 
```
[Nginx这个镜像的Github地址](https://github.com/dylanlindgren/docker-laravel-nginx),其他的我就不放了,因为这个仓库的首页上都有.所以如果你想去详细了解文件内容可以去看看.

# 下载虚拟机并建立共享目录
>要说明一点我用的是windows,所以下面的操作讲的都是windows下的操作经过.OS X 大致类似,linux是最简单的完全不需要虚拟机的,只要建立共享目录就可以了.

  
- 下载docker toolbox并安装 (原文是boot2docker 但是貌似不更新了 而且用不了docker composer 所以用新的吧)
- 建立共享目录并与虚拟机设置共享

```bash
#首先进入 virtualbox的目录 使用命令创建共享目录
VBoxManage sharedfolder add boot2docker-vm --name myapp --hostpath C:\Users\dylan\myapp

#创建转发端口
VBoxManage modifyvm boot2docker-vm --natpf1 "web,tcp,,80,,80"

#当然你还可以直接在virtualbox设置里直接设置这些不用命令 
```
```
#进入linux挂载刚才的共享目录
$ boot2docker ssh 'sudo mkdir /data'
$ boot2docker ssh 'sudo mount -t vboxsf -o "defaults,uid=33,gid=33,rw" myapp /data'

#共享目录我个人感觉没用,因为每次都得进入linux挂载一次可用,而且只用启动容器时 使用 -v 直接挂载目录就可以了 也不需要这个.当然你可以按着步骤一步一步做 这样肯定能成功!
```

# 万事俱备 启动容器

```bash
#启动数据容器 原本好像有点问题 必须这么写要不启动不起来 至于原因嘛 可能跟 bash有关 比较懒没仔细查
`docker run --name myapp-data -v /Users/dylan/myapp:/data:rw -id dylanlindgren/docker-laravel-data /bin/bash ` 

#启动php容器 --privileged=true 这个必须要不无法创建目录
docker run --privileged=true --name myapp-php --volumes-from myapp-data -d dylanlindgren/docker-laravel-phpfpm 

#启动Nginx
docker run --privileged=true --name myapp-web --volumes-from myapp-data -p 80:80 --link myapp-php:fpm -d dylanlindgren/docker-laravel-nginx  


#剩下两个 不想写了 因为感觉没用 如果你想用可以看下面 有原文链接 到那里看一下就会了 跟上面大同小异
```

# 检查浏览器成功与否
进入`http://localhost`查看是否有你想要的内容.这里提个醒因为是laravel环境,所以做了地址改写,把根目录指向了public下,我第一次也是找半天,后来看Github源代码才搞懂. 所以你只要把代码放到你的共享目录,也就是上面C:\Users\dylan\myapp\www\public下就ok了.

# 打完收工
到此为止你有了个全新的laravel环境了.不过这还不是我想要的,毕竟版本都很低了,而且有些设置不太合我的心意,所以会自己弄一个环境出来的.

[想看原文的点这里......](http://dylanlindgren.com/docker-for-the-laravel-framework/)
