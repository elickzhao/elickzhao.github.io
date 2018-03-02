---
title: nginx 下装laravel 
date: 2017-01-04 23:14:51
tags: [服务器相关技术,nginx,laravel]
categories:
---

nginx 配置时 一定要做路由转换,因为laravel使用的是简洁版路由.apache因为在public目录里有 .htaccess所以不用管了, nginx就得自己配置了.

把这句添加进去就哦了
```
location / {
    try_files $uri $uri/ /index.php?$query_string;
}
```


[Laravel Nginx 除 / 外所有路由 404](https://segmentfault.com/q/1010000002422408)
[官方文档](https://laravel.com/docs/5.0/installation#pretty-urls)


