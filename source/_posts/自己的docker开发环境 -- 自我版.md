title: 自己的docker开发环境 -- 自我版
date: 2016-05-04 17:49:40
tags: [docker,服务器相关技术]
categories:
---

这世界变化快啊 我刚学会这个 又出新东西了
[基于Kubernetes构建Docker集群管理详解](http://www.csdn.net/article/2014-12-24/2823292-Docker-Kubernetes)
[Docker Machine + Compose + Swarm](http://www.open-open.com/lib/view/open1428540315869.html#)
[让Docker功能更强大的10个开源工具](http://os.51cto.com/art/201411/456204.htm)
[连接容器 --link 简要说明](http://elickzhao.github.io/2016/05/%E8%BF%9E%E6%8E%A5%E5%AE%B9%E5%99%A8%20--link%20%E7%AE%80%E8%A6%81%E8%AF%B4%E6%98%8E/)




# 懒人的最爱 -- 下载快速用

为了方便急用的同志们,把仓库和配置方法放在最前面,如果想学习的请往下看

- 仓库地址: [https://github.com/elickzhao/docker-study](https://github.com/elickzhao/docker-study)
- 配置方法
1. 进入`dockerfiles`目录, 修改配置文件`docker-compose.yml`
```bash
data:
  build: ./data
  volumes: 
    - "/c/Users/elick/myapp:/data:rw" #这里修改 '/c/Users/elick/myapp' 为你主机上要共享的目录
  privileged: true 
mysql:
  build: ./mysql
  volumes_from:
    - data
  volumes: 
    - "/c/Users/elick/myapp/db/mysql:/var/lib/mysql"  #这里修改 '/c/Users/elick/myapp' 为你主机上要共享的目录
  environment:
    - MYSQL_ROOT_PASSWORD=123456 #根据你的需要修改数据库密码
  ports:
    - "3306:3306"   #根据需要修改数据库端口
php:  
  build: ./php
  expose:
    - "9000"
  volumes_from:
    - data
  links:
    - mysql
  privileged: true

nginx:  
  build: ./nginx
  volumes_from:
    - data
  volumes:
    - "/c/Users/elick/myapp/nginx/nginx.conf:/etc/nginx/nginx.conf" #这里修改 '/c/Users/elick/myapp' 为你主机上要共享的目录
  links:
    - php:php
  ports:
    - "80:80"   #根据需要修改web服务器端口
  privileged: true
```
2. 使用 `docker-composer up` 建立并启动容器 
3. 如果除了data容器 全部启动的话 并且浏览localhost也没错的话 那么你就拥有了最新的php环境了

<!--more-->

- 几点需要注意的
    1.  首先你需要有docker-composer 如果你在windows下 那就下载 docker toolbox 这个默认安装了 linux下的话 我那个仓库里已经下好了
    2.  把仓库克隆下来 并共享此目录 就是替换`/c/Users/elick/myapp` 我这个目录  data容器指向的就是这个目录 指向时一定要是绝对路径 还有就是 windows下 要写虚拟机内的地址 不要写成windows的地址 也就是把此目录已经共享给虚拟机了 因为windows下 容器是运行在虚拟机内的
    3.  目录结构简单说明下
    ```bash
    myapp
    ├─db
    │  └─mysql      #数据保存地址
    ├─dockerfiles
    │  ├─data
    │  ├─mysql
    │  ├─nginx
    │  └─php
    ├─logs          
    ├─nginx         #nginx配置文件放在这里
    │  └─conf.d     #虚拟主机配置文件
    └─www           #网站内容
        └─public
    ```

~~好快速搭建说到这里,喜欢唠叨的可以往下看.~~
#下面不用看了 自己弄时的一些记录 估计没人能看懂 

-----------------------------------

# 心路历程  单一进程容器

**大坑无数的nginx**

单一进程容器,也就是一个服务放在一个容器里

Nginx dockerfile
```
# Apply Nginx configuration
#终于知道这句的用处了 因为每次ip都是变的
#所以不能直接在这里复制到配置出 
#因为第一次改变后 第二次启动就找不到替换的变量了 
#看启动那个shell就明白了
#ADD nginx.conf /opt/etc/nginx.conf
ADD nginx.conf /etc/nginx/nginx.conf


# Nginx startup script
#再一次修改了 因为发现直接用 hosts 里的本地域名就可以指向服务器了
#ADD nginx-start.sh /opt/bin/nginx-start.sh
#RUN chmod u=rwx /opt/bin/nginx-start.sh

RUN mkdir -p /data/nginx/conf.d/
VOLUME ["/data"]


#不能执行 是找不到 sed 命令
#ENTRYPOINT 	sed -i "s/%fpm-ip%/$PHP_PORT_9000_TCP_ADDR/" /etc/nginx/nginx.conf
#ENTRYPOINT /etc/nginx/nginx-start.sh

WORKDIR /opt/bin
#ENTRYPOINT ["/opt/bin/nginx-start.sh"]
#CMD ["true"]
CMD ["nginx"]
```

执行命令建立镜像并启动容器
```
$ docker build -t elick/nginx .
$ docker run --name myapp-nginx -p 80:80 --volumes-from myapp-data -v /c/Users/e
lick/myapp/new/nginx/nginx.conf:/etc/nginx/nginx.conf --link myapp-php:php -d elick
/nginx
```
~~这个nginx有个问题是php-fpm的ip无法得到 必须启动后手动该nginx.conf才行 以前看那个人家使用nginx-start.sh启动 可是到我这命令始终出错 说找不到文件 而且莫名其妙的在每行结尾有个/r的标 而且用#!/bin/bash定义shell也报错 真天了噜~~

~~按道理是 link myapp-php 通过环境变量取得ip 可是就这个shell命令不好使无法进行下去了~~
-p 80 只是指向 容器端口 host端口是随机的


第一个坑,因为需要连接php容器,所以配置文件里需要php服务器的地址,一开始的解决方法是写个shell文件,读取环境变量,因为link以后容器内会包含php容器的环境变量,但是这个shell死活报错,就是下面说的那个问题.

>~~利用命令行已经成功建城环境,不过用docker-composer就遇到问题了~~
问题终于解决了 原来是shell脚本文件的问题 windows下编辑的文本放到linux就会有格式问题 [具体解决看这里](http://elickzhao.github.io/2016/04/%E8%84%9A%E6%9C%AC%E6%8A%A5%E9%94%99%E6%B2%A1%E6%9C%89%E9%82%A3%E4%B8%AA%E6%96%87%E4%BB%B6%E6%88%96%E7%9B%AE%E5%BD%95/)

第二个坑,当时直接把配置文件拷贝到/etc/nginx/nginx.conf 但是每次启动IP都会变化 但是替换变量已经,被替换过了.所以改写了shell 每次都重新复制一遍nginx.conf到/etc/nginx/nginx.conf 然后再去替换才行 

第三个坑,在shell不行的时候,想过用dockerfile执行命令 直接替换,于是发现了 RUN,ENTRYPOINT,CMD 三个命令执行的差别 具体看这里([总结 Dockerfile 一些命令说明](http://elickzhao.github.io/2016/04/%E6%80%BB%E7%BB%93%20Dockerfile%20%E4%B8%80%E4%BA%9B%E5%91%BD%E4%BB%A4%E8%AF%B4%E6%98%8E/)) 但是ENTRYPOINT 执行时 sed命令却没有 不知道为什么没深入研究

经过上面痛苦的经历,最后找到了办法,原来link后会写个静态地址到hosts,所以根本不用那么麻烦,直接在配置文件里写 服务器名称就ok  这个名称就是 --link myapp-php:php 后面php这个别名  具体说明看这里[连接容器 --link 简要说明](http://elickzhao.github.io/2016/05/%E8%BF%9E%E6%8E%A5%E5%AE%B9%E5%99%A8%20--link%20%E7%AE%80%E8%A6%81%E8%AF%B4%E6%98%8E/)


-------------------------------------------------------

**坑少了不少的 php**

```bash
FROM php:fpm
RUN mkdir -p /data
VOLUME ["/data"]

RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng12-dev \
    && docker-php-ext-install -j$(nproc) iconv mcrypt \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    #这个mysqli是我自己添加的 不过这个好像已经下载好了 所以直接install就可以了 没有的恐怕需要上面的apt-get 下载下来才行 不过不知道下载后保存的位置会不会错 因为我自己下了个mysqli 不过在容器里用命令安装时却说找不到
    && docker-php-ext-install -j$(nproc) mysqli


CMD ["php-fpm"]
```

有link 那么 暴露端口其实没用 只要在dockerfile的 EXPOSE 9000 把端口暴露给容器即可  所以没有  -p 9000:9000 暴露端口也没事
```
$ docker build -t elick/php .   #这里可是有个点的啊
$ docker run --name myapp-php -p 9000:9000 --volumes-from myapp-data -d  elick/php

#new
docker run --name myapp-php --link db:mysql --volumes-from myapp-data -d  elick/php
```
这里有一个坑,就是扩展php,因为官方镜像竟然没有mysqli 所以没办法只能自己弄,本来都想放弃了,后来还是平心静气看了下鸟语文档,才发现人家已经准备好命令来解决,只不过我只加了mysqli后没有再深入研究了


**解决上面的问题mysql就没难度了**
```
FROM mysql

MAINTAINER "elick" <xwiwi@foxmail.com>

RUN mkdir -p /data
VOLUME ["/data"]
CMD ["mysqld"]
```

```bash
$ docker build -t elick/mysql .
$ docker run --name myapp-mysql  --volumes-from myapp-data -v  /c/Users/elick/myapp/db/mysql:/var/lib/mysql -d elick/mysql
```

以上都搞懂了 其实mysql就没什么了  其他的相加的服务也是一样

$ docker run --name myapp-data -v /c/Users/elick/myapp/:/data:rw -it elick/data
/bin/bash

------

**用php代码 取得容器环境变量 从而能连接到mysql 可以试一下**
能够获取环境变量 但是只能获取和自己相关的 因为使用的用户 是php那个用户 也就是 www-data 并不是root 所以没法看到完整的env环境变量

我又个下策 就是建立容器时 用shell把环境变量写到 一个文件里 然后读出来  要不然每次启动 连接数据库 还得改php文件 这有点问题啊

蠢死了 原来这么简单 就像link那篇文章里写的 --link 会写入hosts  那么 --link mysql:mysql 那么就会用这个别名 放到 hosts 这样可以直接用这个别名来当服务器名 也就是网址了 

看来想的没错 Nginx 里的ip 也是只要用 php 就可以了  只要yaml里配置的别名是正确的就 哦了 


------
```php
$servername = "mysql";
$username = "root";
$password = "123456";
```
-------




**myapp-data 一点小小的问题**
使用数据容器 用docker-composer 建立时 容器不会启动 但是共享内容还是好使的 究原因 是因为docker是一次性执行 如果没有挂起任务 就会自动关闭 

解决办法 手动建立这个容器 `$ docker run --name myapp-data -v /c/Users/elick/myapp/:/data:rw -it elick/data /bin/bash` 千万不要用 -d  要不容器还是启动不起来 或者自己写个死循环程序 然后启动时 执行这个程序 但是坏处是 当你关闭容器时 会造成麻烦 具体看这篇文章 [通过信号解决docker启动容器后Exited退出的问题](http://xiaorui.cc/2015/01/09/%E9%80%9A%E8%BF%87%E4%BF%A1%E5%8F%B7%E8%A7%A3%E5%86%B3docker%E5%90%AF%E5%8A%A8%E5%AE%B9%E5%99%A8%E5%90%8Eexited%E9%80%80%E5%87%BA%E7%9A%84%E9%97%AE%E9%A2%98/)


~~连接mysql居然还需要本地ip才可以 真实麻烦啊~~


# 另一种形式的环境 就是把nginx和php放到一个容器里 这样省去了很多麻烦
推荐用这个仓库 这个比较好用一些 [https://github.com/skiy-dockerfile/nginx-php7](https://github.com/skiy-dockerfile/nginx-php7)
看来只能使用nginx和php绑定到一起的 因为这样都在同一个服务器 所以使用127.0.0.1 就可以了 而且这种形式也不错 因为根本也没什么必要把他们分开 如有扩展可以前台再加nginx指向这个nginx


# 过去的总结 这个已经过时了
遇到了好多个坑,为了把php-fpm的ip传给nginx,折腾的要死啊! 使用 --link 虽然nginx容器有了 php容器的环境变量IP地址 但是得把这个IP放到nginx.conf里才行 所以需要做些动作

- 刚开始想放到Dockerfile里 用了 `RUN  sed -i "s/%fpm-ip%/$PHP_PORT_9000_TCP_ADDR/" /etc/nginx/nginx.conf` 但是RUN只在image里 执行也就是说此时容器没有启动 所以环境变量还没有
- 于是乎用了 ENTRYPOINT 这个是可以 但是就是上面提到的 shell错误问题 却一直没往这方面想 所以走了弯路
- 其实还用了 CMD 但是好像并没有 执行命令 还得进入容器启动 不知道为什么 也懒得找了