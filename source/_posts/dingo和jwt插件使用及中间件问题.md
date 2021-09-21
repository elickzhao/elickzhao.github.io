---
title: dingo和jwt插件使用及中间件问题
tags: laravel
abbrlink: 2812
date: 2016-11-26 23:42:54
categories:
---

## 简介
在使用的时候,对一个中间件找不到位置.头疼不已,经过一顿寻找终于搞明白了,这里记录下,因为虽然注释了,不过几天后又蒙圈了.前后台一起搞真是玩自己啊.


## 详情

```php
//routes

    // 这里用的中间件 并不是app.php里注册的那个 'auth' => App\Http\Middleware\Authenticate::class,
    // 而是dinggo的api里的middleware就是Auth. 如果用app.php那个会无法验证过期的.
    // 这个注册和jwt一样 都是在 LumenServiceProvider 里完成的 所以不注意会找不到
    $api->group(['middleware' => 'api.auth'], function ($api) {
        // USER
        // my detail
        $api->get('user', [
            'as' => 'user.show',
            'uses' => 'UserController@userShow',
        ]);
    .......

------------


//app

    $app->routeMiddleware([
        'auth' => App\Http\Middleware\Authenticate::class,
        'cors' => App\Http\Middleware\Cors::class,
    ]);



------------

// Dingo\Api\Provider\LumenServiceProvider

     $this->app->routeMiddleware([
        'api.auth' => Auth::class,
        'api.throttle' => RateLimit::class,
        'api.controllers' => PrepareController::class,
    ]);



```
<!--more-->

## 一点小启示

通过dingo和jwt使用有一点小启示.就是服务提供者会在boot()里注册一些中间件或者扩展auth的guards驱动.
看代码吧,说也说不清,估计过几天又会看不懂的.

app.php
```php
// dingo/api
$app->register(Dingo\Api\Provider\LumenServiceProvider::class);
//jwt  
$app->register(Tymon\JWTAuth\Providers\LumenServiceProvider::class);
```

dingo/api
```php
Dingo\Api\Provider;

class LumenServiceProvider extends DingoServiceProvider
{
    /**
     * Boot the service provider.
     *
     * @return void
     */
     public function boot()
    {
        parent::boot();

        $this->app->configure('api');

....
        // 这里就是注册了中间件.
        $this->app->routeMiddleware([
            'api.auth' => Auth::class,
            'api.throttle' => RateLimit::class,
            'api.controllers' => PrepareController::class,
        ]);
    }
```

jwt
```php
class LumenServiceProvider extends AbstractServiceProvider{
    ....
}

//主要看 AbstractServiceProvider

// 73行 这里就是扩展auth 这样在auth配置里就可以使用driver的jwt
    /**
     * Extend Laravel's Auth.
     * 注册扩展jwt
     * @return void
     */
    protected function extendAuthGuard()
    {
        $this->app['auth']->extend('jwt', function ($app, $name, array $config) {
            $guard = new JwtGuard(
                $app['tymon.jwt'],
                $app['auth']->createUserProvider($config['provider']),
                $app['request']
            );

            $app->refresh('request', $guard, 'setRequest');

            return $guard;
        });
    }

```

config/auth
```php
<?php

return [

    'defaults' => [
        'guard' => env('AUTH_GUARD', 'api'),
    ],

    'guards' => [
        'api' => [
            'driver' => 'jwt',  //就是这个了,这就是上面注册的
            'provider' => 'users',
        ],
    ],

```
