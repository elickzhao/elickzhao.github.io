---
title: laravel框架加载流程
tags:
  - laravel
  - php
categories: php开发
abbrlink: 25866
date: 2016-05-26 00:30:10
---


![启动流程](/image/16-5/bootstrap.svg)

<!--more-->

**public/index.php**
>此文件会加载由 Composer 生成的自动加载器定义
并获取由 bootstrap/app.php 文件中所生成的 Laravel 应用程序实例

**bootstrap/app.php**
>生成Laravel应用程序实例 $app
并且绑定核心 App\Http\Kernel::class 或 App\Console\Kernel::class
这取决于请求类型 在这里请求经过一些列操作 最终返回浏览器

**app/Http/Kernel.php**
>一般情况是 HTTP 请求,所以主要说些这个
这个类继承自 Illuminate\Foundation\Http\Kernel 它定义了一个 bootstrappers 数组，在请求被运行前会先行运作。这些启动器设置了错误处理、日志记录、侦测应用程序环境，并运行其它需要在请求实际处理前就该被完成掉的工作。
其实这个过程就是 启动框架的服务提供者 -> Illuminate\Foundation\Bootstrap\BootProviders  这个继承自 Illuminate\Contracts\Foundation\Application  但是实现却是一开始的 Illuminate\Foundation\Application 也就是创建框架实例那个文件里 有这个方法 ~~(有点搞不懂 继承自一个接口,接口没有功能,自己也没有功能,而下面实现这个功能的又没有继承他 那如何找到这个功能的呢)~~ [看这里接口说明](http://elickzhao.github.io/2016/05/%E7%8E%B0%E4%BB%A3%20PHP%20%E6%96%B0%E7%89%B9%E6%80%A7%E7%B3%BB%E5%88%97%20%E2%80%94%E2%80%94%20%E5%96%84%E7%94%A8%E6%8E%A5%E5%8F%A3/#more)
app/Http/Kernel.php
HTTP 核心也定义了一份 HTTP 中间件 清单，所有的请求在被应用程序处理之前都必须经过它们。这些中间件处理 HTTP session 的读写、验证 CSRF 令牌、决定应用程序是否处于维护模式，以及其它更多任务作。

**router**
>所有服务提供者加载完毕后,把request请求转移给router路由器
经过router分派给route(路由)或者controller,并运行中间件处理后返回response(响应)



![启动流程](/image/16-5/2.svg)

整个启动流程,主要涉及的几个文件如下
public/index.php,bootstrap/app.php,app/Http/Kernel.php,
除了Kernel另外两个文件去掉注释,基本很简洁

**public/index.php**
```php
//引入composer自动加载程序
require __DIR__.'/../bootstrap/autoload.php';

//启动框架,生成应用程序
$app = require_once __DIR__.'/../bootstrap/app.php';

//运行应用程序,接收Request请求
$kernel = $app->make(Illuminate\Contracts\Http\Kernel::class);

//经过路由器分发到路由,控制器,中间件的处理返回Response
$response = $kernel->handle(
    $request = Illuminate\Http\Request::capture()
);
//发送响应到浏览器
$response->send();
//销毁请求与响应
$kernel->terminate($request, $response);
```

----------

**bootstrap/app.php**
```php
<?php
//创建应用程序
$app = new Illuminate\Foundation\Application(
    realpath(__DIR__.'/../')
);

//重要的接口绑定 (会根据不同请求,做不同响应.所以主要说下Http)
$app->singleton(
    Illuminate\Contracts\Http\Kernel::class,
    App\Http\Kernel::class
);

$app->singleton(
    Illuminate\Contracts\Console\Kernel::class,
    App\Console\Kernel::class
);

$app->singleton(
    Illuminate\Contracts\Debug\ExceptionHandler::class,
    App\Exceptions\Handler::class
);

//返回应用程序
return $app;

```

----------

**app/Http/Kernel.php**

```php
<?php

namespace App\Http;

use Illuminate\Foundation\Http\Kernel as HttpKernel;

//这里需要说明下,Kernel继承自HttpKernel也就是框架基础核心.
//基础核心主要作用就是,加载这里配置的中间件,还有就是启动框架的各种服务
//因为该文件内容较多就不展示了,可以自己稍微看一下
class Kernel extends HttpKernel
{
    //设置了全局中间件
    protected $middleware = [
        \Illuminate\Foundation\Http\Middleware\CheckForMaintenanceMode::class,
        \App\Http\Middleware\EncryptCookies::class,
        \Illuminate\Cookie\Middleware\AddQueuedCookiesToResponse::class,
        \Illuminate\Session\Middleware\StartSession::class,
        \Illuminate\View\Middleware\ShareErrorsFromSession::class,
        \App\Http\Middleware\VerifyCsrfToken::class,
    ];

    //设置了路由中间件
    protected $routeMiddleware = [
        'auth' => \App\Http\Middleware\Authenticate::class,
        'auth.basic' => \Illuminate\Auth\Middleware\AuthenticateWithBasicAuth::class,
        'guest' => \App\Http\Middleware\RedirectIfAuthenticated::class,
        'test'=> \App\Http\Middleware\TestMiddleware::class,
        'mi'=> \App\Http\Middleware\MiMiddleware::class,
    ];
}
```
结合图片就大致了解,laravel的启动过程.

通过上面注释文件,估计大多数悟性爆表的同学已经明白整个过程了.还有极少数晕乎少年没有明白.这里再多说几句.

其实整个流程就三部 全部在index.php这个文件里

- 启动框架
>这个是由 app.php做到的 并准备好中间 服务待用

- 创建http内核

-  响应请求