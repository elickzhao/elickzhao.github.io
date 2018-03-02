---
title: laravel 修改用户验证为MD5加密方式
date: 2016-11-28 23:00:16
tags: laravel
categories:
---


>真是被这个搞的头都大了.绕来绕去的,到现在AUTH和GUARD倒地是如何分工的,还是不太明白.算了先说怎么弄的吧.

## 解决方案一 简单粗暴

现在用的就是这个方法,也是很多无奈.因为用的JWT插件,一改的话得改很多,而且怎么都得动源代码,不是laravel就是JWT的 所以索性就最简单粗暴吧

Illuminate\Auth\EloquentUserProvider
```php
    // 114行左右
    /**
     * Validate a user against the given credentials.
     *
     * @param  \Illuminate\Contracts\Auth\Authenticatable  $user
     * @param  array  $credentials
     * @return bool
     */
    public function validateCredentials(UserContract $user, array $credentials)
    {
        $plain = $credentials['password'];
        //XXX 自己修改的 md5验证, 这是最快捷的方式,虽然存在隐患,以后再解决吧
        return md5($plain) == $user->getAuthPassword();
        //return $this->hasher->check($plain, $user->getAuthPassword());
    }
```
<!-- more-->

## 解决方案二 看似高大上

第二种方法就是实现 **Hasher** 接口,然后替换掉原来的 **BcryptHasher** 这个类,用自己的加密解密方式.
这个方法的好处是哪天又要改回来很简单.
但有个问题,这TMD不是还是得该源代码,那天要升级 **lumen** 还是有问题,靠.

```php
namespace Illuminate\Contracts\Hashing;

interface Hasher
{

    public function make($value, array $options = []);

    public function check($value, $hashedValue, array $options = []);

    public function needsRehash($hashedValue, array $options = []);
}

//实现这个接口很简单

    public function validateCredentials(UserContract $user, array $credentials)
    {
        $plain = $credentials['password'];
        //return md5($plain) == $user->getAuthPassword();
        //这里的 hasher 的就行了. 不过还是得改代码
        return $this->hasher->check($plain, $user->getAuthPassword());
    }

```

## 解决方案三 完美解决 可惜我这次用不上

这个方法是自己写个服务提供者,继承于 **EloquentUserProvider** 然后修改 **validateCredentials** 方法,这样即使升级也不怕了.完美!
但是我用不了,靠.现在这个系统是结合了 **dingo** 和 **jwt**. 这个验证已经扩展了 **jwt** 已经没法再扩展了. 而且要改会改很多地方.唉....

```php
<?php namespace App\Providers;

use Illuminate\Auth\EloquentUserProvider as BaseEloquentUserProvider;
use Illuminate\Contracts\Auth\Authenticatable;

class EloquentUserProvider extends BaseEloquentUserProvider
{
    public function __construct($model)
    {
        $this->model = $model;
    }

    public function validateCredentials(Authenticatable $user, array $credentials)
    {
        $plain = $credentials['password'];
        $authPassword = $user->getAuthPassword();
        return $authPassword == md5($plain);
    }
}

//---

//注册自定义登录验证方式
//这段代码放在 AuthServiceProvider 的 boot 里,或者 lumen 的 app.php 里

Auth::extend('custom', function() {
   return new EloquentUserProvider('User');
});


//----


//然后修改guard就好了
// config/auth.php

    'guards' => [
        'api' => [
            'driver' => 'custom',
            'provider' => 'users',
        ],
    ],

```


## 后记
真心被这个 Dingo 和 JWT 搞懵了, 有很多乱码七糟的配置不知道是干什么的.跳来跳去的,太复杂了.
下面这个就是, 看似是扩展驱动 jwt 但实际并不是. 扩展驱动,是在 **Tymon\JWTAuth\Providers\LumenServiceProvider** 这个里, 在注册的时候,同时自己注册了很多东西,其中就包括 **guard** 的扩展驱动.

至于下面那个 **Dingo\Api\Auth\Auth** 干什么用的,还是没搞懂.


```php
//jwt   //这里是注册guards > api > driver 下面那个并不是 有点晕.  在 AbstractServiceProvider 73行
$app->register(Tymon\JWTAuth\Providers\LumenServiceProvider::class);
// email 或者放在 provider里面
$app->register(Illuminate\Mail\MailServiceProvider::class);

app('Dingo\Api\Auth\Auth')->extend('jwt', function ($app) {
    return new Dingo\Api\Auth\Provider\JWT($app['Tymon\JWTAuth\JWTAuth']);
});
```

最倒霉的是,做这个时候安装了一个 **debugbar**, 结果不报错 不显示.. 晕死了....

这次一点心得就是,看来md5加密被抛弃了,现在用的是下面这个很简单,而且好像也很强壮
```php

//要加密的密码
$passwod = 123456;

//加密
password_hash($passwod, PASSWORD_DEFAULT);
//解密
password_verify($password, $hash)   // $hash 是加密后的字符串
```

## 参考文档
[请问 Laravel 5 如何更改密码为 md5 加密方式？](https://laravel-china.org/topics/651)
[Laravel5.1自带认证系统（Auth）改变加密方式带来的思考](http://www.jianshu.com/p/450f6a277883)
[PHP处理密码的几种方式](https://segmentfault.com/a/1190000003024932)
[laravel 用户认证](https://laravel-china.org/docs/5.2/authentication)
