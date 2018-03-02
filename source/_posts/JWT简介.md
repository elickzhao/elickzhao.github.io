---
title: JWT简介
date: 2016-12-30 22:23:17
tags: [laravel,php,小百科]
categories:
---

>1. 客户端 token 只存简单的数据，如 userId 。永不过期，除非服务端返回 403 状态码。 
2. 当 token 进来时，校验，解析出 userId ，从缓存（如 redis ）获得 userData ，若缓存存在，更新过期时间。 
3. 若缓存没有命中，从数据库加载返回同时存入缓存，设置过期时间。 
4. 基于安全考虑，可在 token 中再加入一个 version 字段，在第 2 步时校验该段。当改密码或其它需要将所有已登录的客户端重登时只需更新 version 字段，并清空缓存即可。 
以上，客户端无需主动刷新，也无需定期更换 token ，只有在服务端声明 token 无效时抛弃即可，同时里面也不会包含太多的敏感信息（ jwt 只是 base64 编码，随手都可以解析数据出来，不应将敏感数据放在此处），数据量少 token 相对也短些，对传输也有好处；服务端可完全控制 token 的有效性，在必要的情况下可自主 revoke 已颁发的 token 。



## 参考文档
[JWT 简介](http://www.tuicool.com/articles/R7Rj6r3)
[从零实现Lumen-JWT扩展包(序):前因](https://zhuanlan.zhihu.com/p/22531819?refer=lsxiao)
[Lumen上使用Dingo/Api做API开发时用JWT-Auth做认证的实现](http://blog.csdn.net/hooloo/article/details/49714259)
[Laravel 5 中使用 JWT（Json Web Token） 实现基于API的用户认证](http://laravelacademy.org/post/3640.html)
[什么是JWT -- JSON WEB TOKEN](http://www.tuicool.com/articles/me6jua)
[使用json web token](http://www.haomou.net/2014/08/13/2014_web_token/)
[ Laravel (Lumen) 中使用JWT-Auth刷新token的问题](http://blog.csdn.net/hooloo/article/details/50649712)
[自定义中间件（Middleware）监听 Jwt-auth 身份认证（Laravel）](https://www.itlipeng.cn/?p=764)
[ Laravel实现dingo+JWT api接口之实战篇](http://blog.csdn.net/qq_28666081/article/details/52188549)
