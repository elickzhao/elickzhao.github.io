---
title: vue-router的学习笔记
tags: vue
abbrlink: 60925
date: 2017-07-18 23:24:37
categories:
---

模版里链接跳转
```
<router-link to="/foo">Go to Foo</router-link>
```

js跳转方法
```
this.$router.push({ path: '/main' })
```

---

**子页面这么用**

router 配置 

```
const routers = [{
    path: '/',
    meta: {
        title: '后台登录'
    },
    component: (resolve) => require(['./views/login.vue'], resolve) //这个resolve原来是懒加载的意思. 不是一次性加载组件,访问哪个加载哪个
}, {
    path: '/main',
    meta: {
        title: '后台管理中心' //这个是title标题
    },
    component: (resolve) => require(['./views/main.vue'], resolve),
    //这就是子页面了
    children: [
        {
            path: '/',  //当一开始进入页面转到子页面,避免空页面
            //redirect: '/main/sub1'    //指定路径
            redirect:{ name: 'goods' }  //指定命名路径
        },
        {
            path: 'sub1',
            component: (resolve) => require(['./views/sub1.vue'], resolve),
            name:'sub1'
        },
        {
            path: 'sub2',
            component: (resolve) => require(['./views/sub2.vue'], resolve),
            name:'sub2'
        },
        {
            path: 'sub3',
            component: (resolve) => require(['./views/sub3.vue'], resolve),
            name:'sub3'
        },
    ]

}];
```

模版页面使用

```

        .....

            <i-col :span="spanLeft" class="layout-menu-left">
                <Menu active-name="1" theme="light" width="auto">
                    <div class="layout-logo-left">
                        <span class="logo-lg" v-show="spanLeft == 4">
                            <b>Admin</b>LTE
                        </span>
                        <span class="logo-mini" v-show="spanLeft == 1">
                            <b>LTE</b>
                        </span>
                    </div>
                    <Menu-item name="1">
                        <!-- 这里链接一定是 /main/这个下面的子链接 虽然在配置里没这么写  -->
                        <router-link to="/main/sub1" >   
                            <Icon type="ios-navigate" :size="iconSize"></Icon>
                            <span class="layout-text" v-show="spanLeft == 4">选项 1选项</span>
                        </router-link>
                    </Menu-item>
                    <Menu-item name="2">
                        <router-link to="/main/sub2">
                            <Icon type="ios-keypad" :size="iconSize"></Icon>
                            <span class="layout-text">选项 2</span>
                        </router-link>
                    </Menu-item>
                    <Menu-item name="3">
                        <router-link to="/main/sub3">
                            <Icon type="ios-analytics" :size="iconSize"></Icon>
                            <span class="layout-text">选项 3</span>
                        </router-link>
                    </Menu-item>
                </Menu>
            </i-col>
            
            ........
            
            
                    <div class="layout-content-main">
                        <!-- 这里显示子页面 -->
                        <router-view></router-view>
                    </div>
            
            
            
```


## 参考文档
[官方文档](https://router.vuejs.org/zh-cn/essentials/getting-started.html)
[Vue-router2.0学习笔记](https://segmentfault.com/a/1190000007825106)
[vue-router带你出坑](https://segmentfault.com/a/1190000007837361)
[Vue.js——vue-router 60分钟快速入门](http://blog.csdn.net/sinat_17775997/article/details/52549123) `这个是老版文章只能做参考了`  
[Vue 爬坑之路（三）—— 使用 vue-router 跳转页面](http://www.cnblogs.com/wisewrong/p/6277262.html)
