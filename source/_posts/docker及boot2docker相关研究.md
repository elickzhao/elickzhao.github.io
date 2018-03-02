---
title: docker及boot2docker相关研究
date: 2016-04-18 23:51:49
tags: [服务器相关技术,docker]
categories: 服务器端
---
# boot2docker转发端口问题
  这是个很恶心的问题 因为boot2docker还是依赖与virtualbox 所以虽然使用命令 `docker run -dp 8080:8080 php`进行转发 但是还需要修改虚拟机的端口转发才可以 
  > 命令解析:
  docker -d 后台执行 -p 转发端口

  ![虚拟机转发端口](/image/16-4/2.png)

<!--more-->

# boot2docker数据卷问题 
  这个就更恶心了,使用在windows下的boot2docker命令行就会报错,只有进入virtualbox 使用命令 `docker run -dp 8080:8080 --name test1 -v /c/Users/elick/www:/var/www elick/php`才可以 真是醉了
  **需要注意的一点:** boot2docker建立的虚拟机有个自启动共享目录 `c/Users C:\Users` 在虚拟机里的根目录下能看到这个目录 所以windows上的目录的根目录是/c/开始的
![共享目录](/image/16-4/3.png)

# boot2docker使用总结
  从上面两点来看,虽然省略了配置vagrantfile的麻烦,但是还得去弄虚拟机从这点看,貌似还不如使用vagrant来的简单些


# 相关资料
[Docker在PHP项目开发环境中的应用](http://avnpc.com/pages/build-php-develop-env-by-docker)
[{译} 深入理解 Docker Volume（一）](http://www.tuicool.com/articles/uYzeAnz)
[Dockers 快速学习（四）Docker 容器的使用](http://my.oschina.net/mfk123/blog/292425)
[Docker安装_Ubuntu 15.10 & 14.04 LTS上安装和管理Docker](http://www.linuxdown.net/install/soft/2016/0303/4906.html)
[Docker exec与Docker attach](http://blog.csdn.net/halcyonbaby/article/details/46884605)
[Docker之常用命令](http://blog.chinaunix.net/uid-10915175-id-4443127.html)
[Docker学习笔记(2)--Docker常用命令](http://www.tuicool.com/articles/7V7vYn)
[利用Docker构建开发环境](http://tech.uc.cn/?p=2726)
[利用docker快速部署应用](http://snoopyxdy.blog.163.com/blog/static/6011744020147187542090)
[Docker volume使用](http://blog.csdn.net/junjun16818/article/details/30655543)
[Docker学习---挂载本地目录](http://my.oschina.net/piorcn/blog/324202)