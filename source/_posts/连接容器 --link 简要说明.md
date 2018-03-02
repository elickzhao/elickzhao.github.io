title: 连接容器 --link 简要说明
date: 2016-05-2 23:40:34
tags:  docker
---

[docker容器互联的两种方式](http://blog.csdn.net/halcyonbaby/article/details/42112325)
[连接容器 | Docker中文指南](http://get.ftqq.com/26.get)
[Docker多容器连接-以Nginx+PHP为例](https://segmentfault.com/a/1190000002949036)
[Docker学习总结之跨主机进行link](http://www.tuicool.com/articles/RfQRny)
[Docker Machine + Compose + Swarm](http://www.open-open.com/lib/view/open1428540315869.html#)
[让Docker功能更强大的10个开源工具](http://os.51cto.com/art/201411/456204.htm)

这里简单说明下 docker --link 的作用
可以参考下面dockerfile文件.
如果需要连接mysql容器 只要 docker --link mysql 即可 不需要暴露端口 只要dockerfile里有 expose参数 暴露默认端口 3306 就可以了
使用--link 会在hosts文件里 加入一个静态ip地址 以你连接的别名命名的 如果没有别名 就是容器名 例如 mysql 172.17.0.3  所以当你需要连接mysql容器时 只需要在配置文件里写服务器地址为 mysql 就可以了 会自动为你解析成当期容器ip的 因为每次容器启动ip都会变化 根据参考文章说明 此方法试用单个服务器 即所有容器都在一个服务器上 分散的服务器还得需要别的方法 暴露端口或者共享一个连接容器 还有新的方法
就是docker Swarm 创建个集群  还有docker kubernetes 这个google的 前一个是docker自己的  还有这个 Shipyard 好像也不错的样子 唉 东西太多了 根本看不过来啊 
 
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



