---
title: Axios 初探
tags:
  - js
  - vue
abbrlink: 42492
date: 2017-08-01 20:07:38
categories:
---

**简单用法**

```js
/* 向具有指定ID的用户发出请求 */
//其实和jq的ajax差不多 then 是成功返回执行其后任务,catch捕捉错误
//其实就是 Promise 可以进行并发操作的 axios.all(iterable) 其实就是 promise.all()
axios.get('/user?ID=12345')
  .then(function(response){
    console.log(response);
  })
  .catch(function(error){
    console.log(error);
  });


```

**还有以下用法**
```js
axios.request(config)
axios.get(url [，config])
axios.delete(url [，config])
axios.head(url [，config])
axios.post(url [，data [，config]])
axios.put(url [，data [，config]])
axios.patch(url [，data [，config]])
```


**创建实例使用**

```js
const ajaxUrl = env === 'development' ?
    'http://127.0.0.1:3000' :
    env === 'production' ?
    'https://www.url.com' :
    'https://debug.url.com';

util.ajax = axios.create({
    baseURL: ajaxUrl,
    timeout: 30000
});


//这样使用  这样就不用每次都写那个很长的请求地址了
    Util.ajax.get('users', {  //只能用get 是请求 post是创建
        params: {
        user: this.formInline.user,
        password: this.formInline.password
        }
    })

```


## 参考文档
[Axios全攻略](http://www.tuicool.com/articles/eMb2yuY)
[axios基本用法](http://www.cnblogs.com/Upton/p/6180512.html)
