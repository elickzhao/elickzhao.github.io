title: Dockerfile VOLUME 和 -v 的区别
date: 2016-04-21 21:27:05
tags: ['docker','服务器相关技术']
categories: 服务器端
---


elick     发布    新文稿         
    
Dockerfile VOLUME 和 -v 的区别

docker 服务器相关技术

找到了官方的userguide： https://docs.docker.com/engine/userguide...

从中了解了一点：VOLUME并非只是声明，它会把指定路径重新加载一遍，我通过inspect容器也发现了这一点。

这是在Dockerfile指定了VOLUME，并没有指定-v，查看容器的Mounts信息：
```yml
"Mounts": [
        {
            "Name": "b3e2dcacd3f9f40b43ccd5773d45ca74f0f49b02d3da17749cb378ff9f59bb67",
            "Source": "/var/lib/docker/volumes/b3e2dcacd3f9f40b43ccd5773d45ca74f0f49b02d3da17749cb378ff9f59bb67/_data",
            "Destination": "/etc",
            "Driver": "local",
            "Mode": "",
            "RW": true
        }
    ],
```
这是在上一个的基础上，指定了-v，查看容器的Mounts信息：

```yml
 "Mounts": [
        {
            "Source": "/etc",
            "Destination": "/etc",
            "Mode": "",
            "RW": true
        }
    ],
```

然后你去`/var/lib/docker/volumes/b3e2dcacd3f9f40b43ccd5773d45ca74f0f49b02d3da17749cb378ff9f59bb67/_data`目录下看一下，大致就清楚了。当Dockerfile里设置的VOLUME目录存在文件,那么在这个目录里就会出现 如用用了-v再指向这个VOLUME目录,又会被host目录覆盖掉

你可以把VOLUME理解为，从镜像中复制指定卷的文件夹到本地/var/lib/docker/volumes/xxxxxxxxx/文件夹，然后把本地的该文件夹挂载到容器里面去。

本质上还是相当于一个本地文件夹挂载而已。

继续补充，因为VOLUME实际上就是在本地新建了一个文件夹挂载了，那么实际上容器内部的文件夹有三种情况： 
1、没有指定VOLUME也没有指定-v，这种是普通文件夹。 
2、指定了VOLUME没有指定-v，这种文件夹可以在不同容器之间共享，但是无法在本地修改。 
3、指定了-v的文件夹，这种文件夹可以在不同容器之间共享，且可以在本地修改。

那就列举一种需要在不同容器之间共享且不需要在本地修改的情况。

首先，我们先了解容器中获取动态数据的方式： 
1、本地提供，挂载到容器 
2、远程提供，从远程下载 
3、生成提供，在容器内部生成

后面两种命令都不需要在本地修改，但是他们生成的动态数据却可能需要共享。 
下载命令，比如git clone直接从git服务器拉取代码，不需要挂载本地文件夹。 
生成命令，比如jekyll（静态网站生成器），你可能挂载一个代码文件夹，然后build目录里生成的静态网页文件需要提供给Apache服务器，那么你需要指定build目录为VOLUME。

