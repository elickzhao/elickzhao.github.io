title: Docker常用命令
tags:
  - docker
  - 服务器相关技术
categories:
  - 服务器端
date: 2016-04-19 00:42:00
---

# 查看docker信息（version、info）
```bash
# 查看docker版本  
$docker version  
  
# 显示docker系统的信息  
$docker info  
```

# 对image的操作（search、pull、images、rmi、history）

```bash
# 检索image  
$docker search image_name  
  
# 下载image  
$docker pull image_name  
  
# 列出镜像列表; -a, --all=false Show all images; --no-trunc=false Don't truncate output; -q, --quiet=false Only show numeric IDs  
$docker images  
  
# 删除一个或者多个镜像; -f, --force=false Force; --no-prune=false Do not delete untagged parents  
$docker rmi image_name  

#删除所有镜像
docker rmi $(docker images -q)
docker rmi $(docker images | grep none | awk '{print $3}' | sort -r)

#删除所有未打 tag 的镜像
docker rmi $(docker images -q | awk '/^<none>/ { print $3 }')

#根据格式删除所有镜像
docker rm $(docker ps -qf status=exited)
  
# 显示一个镜像的历史; --no-trunc=false Don't truncate output; -q, --quiet=false Only show numeric IDs  
$docker history image_name 

#导出容器到本地镜像库：
$docker export container_id > centos.tar


#导入容器快照为镜像(docker import)：
#(1)容器在本地：
cat centos.tar | docker import - registry.intra.weibo.com/yushuang3/centos:v2.0

#(2)容器在网络上：
docker import http://example.com/exampleimage.tgz registry.intra.weibo.com/yushuang3/centos:v2.0

Note：
用户既可以使用 docker load 来导入镜像存储文件到本地镜像库，
也可以使用 docker import 来导入一个容器快照到本地镜像库。
这两者的区别在于容器快照文件将丢弃所有的历史记录和元数据信息（即仅保存容器当时的快照状态），
而镜像存储文件将保存完整记录，体积也要大。此外，从容器快照文件导入时可以重新指定标签等元数据信息。

```


# 启动容器（run）

docker容器可以理解为在沙盒中运行的进程。这个沙盒包含了该进程运行所必须的资源，包括文件系统、系统类库、shell 环境等等。但这个沙盒默认是不会运行任何程序的。你需要在沙盒中运行一个进程来启动某一个容器。这个进程是该容器的唯一进程，所以当该进程结束的时候，容器也会完全的停止。

```bash
# 在容器中运行"echo"命令，输出"hello word"  
$docker run image_name echo "hello word"  

#命名容器
$docker run --name test image_name
Note: 这个很有用 这样删除容器等一些操作直接用名称就可以 不用去看ID了
  
# 交互式进入容器中  
$docker run -i -t image_name /bin/bash  
Note: 如果镜像有tag，需要在image后加:tag名 （-t 选项让Docker分配一个伪终端（pseudo-tty）并绑定到容器的标准输入上， -i 则让容器的标准输入保持打开）

#后台执行容器
$docker run -d image_name 

#转发端口
$docker run -p 8080:8080 image_name  
Note:  主机端口(host port) : 容器端口(container post)

#挂载指定的主机目录
$ docker run -v /c/Users/elick/www:/var/www
Note: 主机目录 : 容器目录 挂载指定主机目录 这个是Dockerfile VOLUME没法办到的 因为考虑的Dockerfile的迁移问题 主机目录是不确定的,所这个指定目录只能用命令行来执行 而Dockerfile VOLUME 只是挂载了容器内一个目录 还有个问题是 这个挂载是吧主机目录完全复制到容器目录 但不是双向的 会把容器目录内容删除 **两个地址都必须是绝对地址 还有容器内的目录可以没有 会自动添加的**
  
# 在容器中安装新的程序  
$docker run image_name apt-get install -y app_name  

Note：  在执行apt-get 命令的时候，要带上-y参数。如果不指定-y参数的话，apt-get命令会进入交互模式，需要用户输入命令来进行确认，但在docker环境中是无法响应这种交互的。apt-get 命令执行完毕之后，容器就会停止，但对容器的改动不会丢失。

# container内的root拥有真正的root权限。
docker run --privileged=false         

Note: 使用该参数，container内的root拥有真正的root权限。
否则，container内的root只是外部的一个普通用户权限。
否则，container内的root只是外部的一个普通用户权限。
privileged启动的容器，可以看到很多host上的设备，并且可以执行mount。
甚至允许你在docker容器中启动docker容器。
```

# 查看容器（ps）

```bash
# 列出当前所有正在运行的container  
$docker ps  
# 列出所有的container  
$docker ps -a  
# 列出最近一次启动的container  
$docker ps -l  
```




# 保存对容器的修改（commit）

当你对某一个容器做了修改之后（通过在容器中运行某一个命令），可以把对容器的修改保存下来，这样下次可以从保存后的最新状态运行该容器。

```bash
# 保存对容器的修改; -a, --author="" Author; -m, --message="" Commit message  
$docker commit -a 'author' ID new_image_name  


Note：  image相当于类，container相当于实例，不过可以动态给实例安装新软件，然后把这个container用commit命令固化成一个image。
```




# 对容器的操作（rm、stop、start、kill、logs、diff、top、cp、restart、attach、exec、rename、logs、link）

```bash
# 删除所有容器  
$docker rm `docker ps -a -q`  
  
# 删除单个容器; -f, --force=false; -l, --link=false Remove the specified link and not the underlying container; -v, --volumes=false Remove the volumes associated to the container  
$docker rm Name/ID  
  
# 停止、启动、杀死一个容器  
$docker stop Name/ID  
$docker start Name/ID  
$docker kill Name/ID  
  
# 从一个容器中取日志; -f, --follow=false Follow log output; -t, --timestamps=false Show timestamps  
$docker logs Name/ID  
  
# 列出一个容器里面被改变的文件或者目录，list列表会显示出三种事件，A 增加的，D 删除的，C 被改变的  
$docker diff Name/ID  
  
# 显示一个运行的容器里面的进程信息  
$docker top Name/ID  
  
# 从容器里面拷贝文件/目录到本地一个路径  
$docker cp Name:/container_path to_path  
$docker cp ID:/container_path to_path  
  
# 重启一个正在运行的容器; -t, --time=10 Number of seconds to try to stop for before killing the container, Default=10  
$docker restart Name/ID  
  
# 附加到一个运行的容器上面; --no-stdin=false Do not attach stdin; --sig-proxy=true Proxify all received signal to the process  
$docker attach ID  

Note： attach命令允许你查看或者影响一个运行的容器。你可以在同一时间attach同一个容器。你也可以从一个容器中脱离出来，是从CTRL-C。但是脱离出来后 容器也就停止了

#进入容器
$docker exec -it container_name /bin/bash
Note: exec 使用-it时，和我们平常操作console界面类似。而且也不会像attach方式因为退出，导致 
整个容器退出。（-t 选项让Docker分配一个伪终端（pseudo-tty）并绑定到容器的标准输入上， -i 则让容器的标准输入保持打开） 和run -it 一样后面必须接上要执行的命令 比如 /bin/bash 否则会报错

#修改容器名
$docker rename old容器名  new容器名

#要获取容器的输出信息:
$docker logs -f <容器名orID>

#查看容器的root用户密码
$docker logs <容器名orID> 2>&1 | grep '^User: ' | tail -n1

#一个容器连接到另一个容器
$docker run -i -t --name sonar -d -link mmysql:db   tpires/sonar-server
Note: sonar容器连接到mmysql容器，并将mmysql容器重命名为db。这样，sonar容器就可以使用db的相关的环境变量了。如下查看环境变量就可以看到mmysql相关环境变量


root@665f1bdc5913:/# env
HOSTNAME=665f1bdc5913
DB_NAME=/compassionate_pasteur/db
TERM=xterm
DB_PORT=tcp://172.17.0.2:11211
DB_PORT_3306_TCP_PROTO=tcp
DB_PORT_3306_TCP_ADDR=172.17.0.2
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
PWD=/
SHLVL=1
HOME=/
DB_PORT_3306_TCP_PORT=3306
DB_PORT_3306_TCP=tcp://172.17.0.2:3306
container=lxc
_=/usr/bin/env

```




# 保存和加载镜像（save、load）


当需要把一台机器上的镜像迁移到另一台机器的时候，需要保存镜像与加载镜像。
```bash
# 保存镜像到一个tar包; -o, --output="" Write to an file  
$docker save image_name -o file_path  
# 加载一个tar包格式的镜像; -i, --input="" Read from a tar archive file  
$docker load -i file_path  
  
# 机器a  
$docker save image_name > /home/save.tar  
# 使用scp将save.tar拷到机器b上，然后：  
$docker load < /home/save.tar  
```

# 登录registry server（login）

```bash
# 登陆registry server; -e, --email="" Email; -p, --password="" Password; -u, --username="" Username  
$docker login  
```

# 发布image（push）

```bash
# 发布docker镜像  
$docker push new_image_name  
```

# 根据Dockerfile 构建出一个容器
```bash
#build  
      --no-cache=false Do not use cache when building the image  
      -q, --quiet=false Suppress the verbose output generated by the containers  
      --rm=true Remove intermediate containers after a successful build  
      -t, --tag="" Repository name (and optionally a tag) to be applied to the resulting image in case of success  
$docker build -t image_name Dockerfile_path  
```


[Docker 2 -- 关于Dockerfile](http://blog.tankywoo.com/docker/2014/05/08/docker-2-dockerfile.html)
[Docker 4 -- 总结](http://blog.tankywoo.com/docker/2014/05/08/docker-4-summary.html)
[深入理解 Docker Volume（一）](http://www.tuicool.com/articles/uYzeAnz)
[docker 容器相关命令](http://blog.csdn.net/qinyushuang/article/details/43342091)