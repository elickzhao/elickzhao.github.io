---
title: Docker Compose 配置文件 yaml  参数简单介绍
tags: docker
abbrlink: 22562
date: 2016-05-02 23:35:58
categories:
---


一般情况的用法
```
mysql:
  build: ./mysql
  volumes_from:
    - myapp-data 
  environment:
        - MYSQL_ROOT_PASSWORD=123456
php:  
  build: ./php
  expose:
    - "9000:9000"
  volumes_from:
    - myapp-data
  links:
    - mysql
  privileged: true

nginx:  
  build: ./nginx
  volumes_from:
    - myapp-data
  links:
    - php:php
  ports:
    - "80:80"
  privileged: true
```
<!--more-->

|配置参数|说明|
|---|:---|
|image|从镜像的构建容器|
|build|直接从pwd的Dockerfile来build，而非通过image选项来pull|
|links|连接到那些容器。每个占一行，格式为SERVICE[:ALIAS],例如 – db[:database] 如果不写别名 就是一致的 这个别名 会写在hosts里 当作服务器名 这样可以直接用于连接容器服务|
|external_links|连接到该compose.yaml文件之外的容器中，比如是提供共享或者通用服务的容器服务。格式同links|
|command|替换默认的command命令|
|ports|导出端口 主机端口:容器端口|
|expose|导出端口，但不映射到宿主机的端口上。它仅对links的容器开放。格式直接指定端口号即可。|
|volumes|加载路径作为卷, 主机路径:容器路径:rw(读写属性) 容器路径必须是绝对路径|
|volumes_from|加载其他容器 服务所有卷|
|env_file|从一个文件中导入环境变量，文件的格式为RACK_ENV=development|
|extends|扩展另一个服务，可以覆盖其中的一些选项 看下面 例1 |
|net|容器的网络模式，可以为”bridge”, “none”, “container:[name or id]”, “host”中的一个。|
|dns|可以设置一个或多个自定义的DNS地址。|
|dns_search|可以设置一个或多个DNS的扫描域。|
|orking_dir, entrypoint, user, hostname, domainname, mem_limit, privileged, restart, stdin_open, tty, cpu_shares，docker run|这些命令都是单行的命令,效果和Dockerfile是一样的  看例2|
例1
```
common.yml
webapp:
  build:./webapp
  environment:- DEBUG=false- SEND_EMAILS=false

----------------------------------------------------------

development.yml
web:extends:
    file: common.yml
    service: webapp
  ports:-"8000:8000"
  links:- db
  environment:- DEBUG=true
db:
  image: postgres
```

例2 
```
cpu_shares:73
working_dir:/code
entrypoint: /code/entrypoint.sh
user: postgresql
hostname: foo
domainname: foo.com
mem_limit:1000000000
privileged:true
restart: always
stdin_open:true
tty:true
```

常用命令说明

|命令参数|说明|
|---|---|
|docker-compose up| 启动服务器| 
|-d |以daemon(守护进程)方式启动容器
|--verbose|输出详细信息
|-f|制定一个非docker-compose.yml命名的yaml文件
|-p|设置一个项目名称（默认是directory名）
|build|构建服务
|kill -s SIGINT|给服务发送特定的信号
|logs|输出日志
|port|输出绑定的端口
|ps|输出运行的容器
|pull|pull服务的image
|rm|删除停止的容器
|run|运行某个服务，例如docker-compose run web python manage.py shell
|start|运行某个服务中存在的容器
|stop|停止某个服务中存在的容器
|up|create + run + attach容器到服务
|scale|设置服务运行的容器数量。例如：docker-compose scale web=2 worker=3

感觉有些命令和docker命令重合 所以感觉有些命令没什么用处

[Docker Compose YAML 模板文件](http://codecloud.net/docker-compose-yaml-5232.html)