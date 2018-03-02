title: 总结 Dockerfile 一些命令说明
date: 2016-04-27 23:44:57
tags: [docker,服务器相关技术]
categories: 服务器端
---

>Dockerfile是一个镜像的表示，可以通过Dockerfile来描述构建镜像的步骤，并自动构建一个容器
所有的 Dockerfile 命令格式都是:
INSTRUCTION arguments
虽然指令忽略大小写，但是建议使用大写。

# FROM 命令
```
FROM <image>
```
或
```
FROM <image>:<tag>
```
这个设置基本的镜像，为后续的命令使用，所以应该作为Dockerfile的第一条指令。

比如:
```
FROM ubuntu
```
如果没有指定 tag ，则默认tag是latest，如果都没有则会报错。




# CMD 命令

有三种格式:
```
CMD ["executable","param1","param2"] (like an exec, preferred form)
CMD ["param1","param2"] (as default parameters to ENTRYPOINT)
CMD command param1 param2 (as a shell)
```
一个Dockerfile里只能有一个CMD，如果有多个，只有最后一个生效。这是为了引用镜像时避免启动服务器而无法配置 所以你可以在最后再写个CMD从而屏蔽原镜像的命令

<!--more-->

# RUN 命令


RUN命令会在上面FROM指定的镜像里执行任何命令，然后提交(commit)结果，提交的镜像会在后面继续用到。

两种格式:
```
RUN <command> (the command is run in a shell - `/bin/sh -c`)
```
或:
```
RUN ["executable", "param1", "param2" ... ]  (exec form)
#RUN命令等价于:

docker run image command
docker commit container_id
```


#ENTRYPOINT 命令

有两种语法格式，一种就是上面的(shell方式):
```
ENTRYPOINT cmd param1 param2 ...
```
第二种是 exec 格式:
```
ENTRYPOINT ["cmd", "param1", "param2"...]
```
如:
```
ENTRYPOINT ["echo", "Whale you be my container"]
```
ENTRYPOINT 命令设置在容器启动时执行命令 这样 --link 的其他容器环境变量用这个命令将会取得
>这个可能是 和 RUN最大区别 RUN是生成容器时执行一次,还有个区别就是 ENTRYPOINT 和 CMD 一样只执行最后一个  RUN是可以执行多次的
~~有一次使用sed命令时 RUN可以用 ENTRYPOINT 虽然能生成容器但是却启动不起来 那次用的Docker-composer 所以也不知道和那个是否有关~~
```
root@tankywoo-docker:~# cat Dockerfile
FROM ubuntu
ENTRYPOINT echo "Welcome!"

root@tankywoo-docker:~# docker run 62fda5e450d5
Welcome!
```
启动几次执行几次 最上面 Step 7 哪行命令
![启动执行ENTRYPOINT](/image/16-4/6.png)



>**简要说明:**
RUN是在building image时会运行的指令, 在Dockerfile中可以写多条RUN指令.
CMD和ENTRYPOINT则是在运行container 时会运行的指令, 都只能写一条, 如果写了多条, 则最后一条生效.
CMD和ENTRYPOINT的区别是: 
CMD在运行时会被command覆盖, ENTRYPOINT不会被运行时的command覆盖, 但是也可以指定.



# ADD 命令

从src复制文件到container的dest路径:
```
ADD <src> <dest>
```
`<src>` 是相对被构建的源目录的相对路径，可以是文件或目录的路径，也可以是一个远程的文件url
`<dest>` 是container中的绝对路径


# VOLUME 命令
```
VOLUME ["<mountpoint>"]
```
如:
```
VOLUME ["/data"]
```
创建一个挂载点用于共享目录

#USER 命令

比如指定 memcached 的运行用户，可以使用上面的 ENTRYPOINT 来实现:
```
ENTRYPOINT ["memcached", "-u", "daemon"]
```
更好的方式是：
```
ENTRYPOINT ["memcached"]
USER daemon
```


#MAINTAINER 命令

```
MAINTAINER <name>
```
MAINTAINER命令用来指定维护者的姓名和联系方式


# EXPOSE 命令

EXPOSE 命令可以设置一个端口在运行的镜像中暴露在外
```
EXPOSE <port> [<port>...]
```
比如Nginx使用端口 80，可以把这个端口暴露在外，这样容器外可以看到这个端口并与其通信。
```
EXPOSE 80
```
详细内容可以看这篇文章 -> [Docker网络原则入门：EXPOSE，-p，-P，-link](http://www.open-open.com/lib/view/open1435126385232.html)


# WORKDIR 命令
```
WORKDIR /opt/bin
```
配置RUN, CMD, ENTRYPOINT 命令设置当前工作路径

可以设置多次，如果是相对路径，则相对前一个 WORKDIR 命令

比如:
```
WORKDIR /opt/bin
RUN pwd
```
其实是在 /opt/bin 下执行 pwd


# ENV 命令

用于设置环境变量
```
ENV <key> <value>
```
设置了后，后续的RUN命令都可以使用

使用此dockerfile生成的image新建container，可以通过 docker inspect 看到这个环境变量:
```
root@tankywoo-docker:~# docker inspect 49bfc7a9817f
    ...
    "Env": [
        "name=tanky",
        "HOME=/",
        "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
    ],
    ...
```    
里面的name=tanky就是设置的。

也可以通过在docker run时设置或修改环境变量:
```
docker run -i -t --env name="tanky" ubuntu:newtest /bin/bash
```