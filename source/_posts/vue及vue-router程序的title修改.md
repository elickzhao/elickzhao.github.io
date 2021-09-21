---
title: vue及vue-router程序的title修改
tags: vue
abbrlink: 38232
date: 2017-07-22 14:02:38
categories:
---

因为`vue`控制的是`body`部分,所以没法修改`header`里的`title`这个比较麻烦而且不美观.

现在新的 `vue-router` 可以解决这个问题了.用钩子方法来做.


```js
//这是钩子进入页面之前,就修改title
router.beforeEach((to, from, next) => {
  document.title = to.meta.title    //这 to.meta.title 是在router里设置的
  next()
})
```

这是vue-router设置
```js
const routers = [{
    path: '/',
    meta: {
        title: '后台登录'   //这个就是上面设置
    },
    component: (resolve) => require(['./views/login.vue'], resolve)
},{
    path: '/main',
    meta: {
        title: '后台管理中心' //这个是title标题
    },
    component: (resolve) => require(['./views/main.vue'], resolve)

}];

```

## 参考文档
[vue-router2.0设置title](https://segmentfault.com/q/1010000007739026?_ea=1441360)
