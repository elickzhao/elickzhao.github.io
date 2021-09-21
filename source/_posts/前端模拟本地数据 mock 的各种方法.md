---
title: 前端模拟本地数据 mock 的各种方法
tags:
  - 前端
  - js
  - node.js
  - mock
abbrlink: 52628
date: 2017-08-19 21:40:01
categories:
---

**[json-server](https://yarnpkg.com/zh-Hans/package/json-server)**
通过REST路由操作JSON文件数据库

**[JSONPlaceholder](https://yarnpkg.com/zh-Hans/package/jsonplaceholder)**
用于测试和原型的简单假REST API服务器。

**[fake-api-server](https://yarnpkg.com/zh-Hans/package/fake-api-server)**
跟JSONPlaceholder一样,是用于测试信息的测试服务器

**[ssr](https://github.com/jaywcjlove/ssr)**
这个和上面的 **json-server** 异曲同工之妙,这不过这个既可以当作静态网站服务器,也可以提供 **本地数据mock** 用于测试开发 这个可以配合mock.js 利用代理路由 达到模拟数据 这个方法简单些 比上面的写json文件好点

**[mock.js](http://mockjs.com/examples.html)**
这个有个问题拦截ajax只能是jq,还有个插件是angular的 可惜我不用.
我只用vue,虽然也可以手动添加但还是不如插件简单,以后可以考虑自己搞一个.
对了可以配合另一个插件 **axios-mock-adapter** 做到拦截并返回模拟数据
[我自己的文章说明](https://www.zybuluo.com/mdeditor#800670)
