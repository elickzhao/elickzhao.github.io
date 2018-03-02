---
title: laravel核心理解 ServiceProvider(服务提供者) Container(容器) Facades(门面)
date: 2017-01-13 22:33:21
tags: [laravel,php]
categories:
---


时间紧迫先简单说明下
>**服务提供者:** 是绑定服务到容器的工具.

>**容器:** 是laravel装载服务提供者提供的服务实例的集合或对象

>**门面:** 使用这些服务的快捷方式.也就是静态方法.

>**注意:** 如果一个类没有基于任何接口那么就没有必要将其绑定到容器。容器并不需要被告知如何构建对象，因为它会使用 PHP 的反射服务自动解析出具体的对象。(?后面这个明白意思,但不知道怎么做)

<!--more-->
## Provider (服务提供者)
### 主要作用就是绑定服务到容器中,看下面例子.
>在一个服务提供者中，可以通过 $this->app 变量访问容器，然后使用 bind 方法注册一个绑定，该方法需要两个参数，第一个参数是我们想要注册的类名或接口名称，第二个参数是返回类的实例的闭包

```php
<?php

namespace App\Providers;

use Riak\Connection;
use Illuminate\Support\ServiceProvider;

class RiakServiceProvider extends ServiceProvider
{
    /**
     * 在容器中注册绑定。
     *
     * @return void
     */
    public function register()
    {   //这是5.3写法
        $this->app->singleton(Connection::class, function ($app) {
            return new Connection(config('riak'));
        });
        //5.2以前这么写 $this->app->singleton('Riak\Contracts\Connection', function ($app){...}
        // Connection::class 返回的就是命名空间字符串(可以看另一个我的文章),所以跟5.2是一样的.  
        //这里千万注意'Riak\Contracts\Connection'是个字符串,相当于在容器中定义了一个名字并把实例赋值给了这个名字.
        //并没有其他更高深的含义
        //所以只要在 config/app 下的provider数组里 注册一下就可以使用了
    }
}
```

**但是最经常用的方式是:**
>服务容器的一个非常强大的特性是其绑定接口到实现的能力。我们假设有一个 EventPusher 接口及其 RedisEventPusher 实现，编写完该接口的 RedisEventPusher 实现后，就可以将其注册到服务容器：
```php
//EventPusher 为契约接口
//RedisEventPusher 为实现这个接口的redis工具
$this->app->bind('App\Contracts\EventPusher', 'App\Services\RedisEventPusher');
```

**这样做的妙处在于这里:**
>这段代码告诉容器当一个类需要 EventPusher 的实现时将会注入 RedisEventPusher，现在我们可以在构造器或者任何其它通过服务容器注入依赖的地方进行 EventPusher 接口的类型提示,就会把RedisEventPusher的实例传给类

>还有个好处就是,当想要换掉EventPusher,很方便你不需要改别的地方的代码只要在provider把绑定类换掉就可以了,换成另一个实现了 EventPusher 类就ok了. 当然还有更强大一个接口绑定多个实现,然后根据条件选择使用的类,这个 laravel 也能做到.
```php
use App\Contracts\EventPusher;

/**
 * 创建一个新的类实例
 *
 * @param  EventPusher  $pusher
 * @return void
 */
public function __construct(EventPusher $pusher){
    $this->pusher = $pusher;
}
```

### 绑定到容器后,就是注册到Provider,让 laravel 可以自动加载
这次很简单 就是添加到 config/app 下的 Provider数组里
```php
    'providers' => [

        /*
         * Laravel Framework Service Providers...
         */
        Illuminate\Auth\AuthServiceProvider::class,
        Illuminate\Broadcasting\BroadcastServiceProvider::class,
        Illuminate\Bus\BusServiceProvider::class,
        Illuminate\Cache\CacheServiceProvider::class,
.... 
    ],
```

### 然后就是使用了
其实上面已经说过了 使用最长用的方法就是 __construct()里注入
```php
use App\Contracts\EventPusher;

/**
 * 创建一个新的类实例
 *
 * @param  EventPusher  $pusher
 * @return void
 */
public function __construct(EventPusher $pusher){
    $this->pusher = $pusher;
}

public function test(){
    //当然还可以手动调用 
    $fooBar = $this->app->make('FooBar');
    //其次，你可以以数组方式访问容器，因为其实现了 PHP 的 ArrayAccess 接口：
    $fooBar = $this->app['FooBar'];
}
```


## 容器说明
容器很简单 就是$this->app 或者帮助函数app()获得就是laravel实例 里面就是容器

## 门面说明
有空具体写一下, 门面有两个情形,一种是Cache类的,一种是Config类的. Config比较简单就是实现了一个接口,然后关联一个服务. 而且好像是laravel自动加载的,不需要手动在config/app的数组里添加. 
但是Cache就不同了,他必须在config/app里手动添加.而且他还有一个服务提供者Provider,在容器里的'cache'其实首先指定的是这个Provider,并不像Config那么简单直接指向服务了.

这是门面类, 这里的'cache'是容器里的实例. 这个在Config/app的alias里.  但你会发现Cache绑定容器的Provider,帮的'cache'是个管理类. 但是Config却找不到这个Provider在哪里.他只有简单的继承了一个接口就完了.
```php
class Cache extends Facade{
    /**
     * 获取组件注册名称
     *
     * @return string
     */
    protected static function getFacadeAccessor() { 
        return 'cache'; 
    }
}
```