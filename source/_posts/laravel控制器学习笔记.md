---
title: laravel控制器学习笔记
date: 2016-06-05 23:54:43
tags: [laravel,php]
categories: php开发
---


**一些私有属性说明**
```
    //注册成功后挑战地址
    protected $redirectPath = '/';
    //登录成功后跳转地址
    protected $loginPath = '/';
    //但是有个问题当登录成功后 还是进入auth/login的时候 还是会默认跳转到/home路径下
    
    //重置密码后跳转路径
    protected $redirectTo = '/dashboard';
    //设置这个属性就可以设置登录验证的字段
    protected $username = 'name';
    
    //自定义登录模版位置
    protected $loginView = 'admin.auth.login';
    //自定义注册模版位置
    protected $registerView = 'admin.auth.register';
    //这个自定义验证配置项 在config/auth.php 但是这是5.2属性 5.1没有  
    //这个可以坐到对前台会员登录和后台管理员登录 分离管理
    protected $guard = 'admin';
```

