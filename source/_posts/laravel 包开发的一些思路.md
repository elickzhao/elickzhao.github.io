---
title: laravel 包开发的一些思路
tags:
  - php
  - laravel
categories: php开发
abbrlink: 448
date: 2016-05-21 22:52:28
---


- ContactServiceProvider 服务提供者
 > 这里面 registerContact() 其实是没用的 应该是调用其他服务功能时才有用.
但是这个简单功能完全可通过router和controller完全能实现所以 感觉不需要写个什么服务了.
暂时这么理解吧

```php
<?php namespace Jai\Contact;

use Illuminate\Support\ServiceProvider;
use Illuminate\Routing\Router;

class ContactServiceProvider extends ServiceProvider
{
    protected $defer = false;
    public function boot()
    {
        //注册模版地址 这里一定要使用realpath() 不是绝对路径就会出错
        $this->loadViewsFrom(realpath(__DIR__.'/../views'), 'contact');
        //注册包路由
        $this->setupRoutes($this->app->router);
        // this for conig
        $this->publishes([
            __DIR__.'/config/contact.php' => config_path('contact.php'),
        ]);
    }

    /**
     * Define the routes for the application.
     *
     * @param \Illuminate\Routing\Router $router
     * @return void
     */
    public function setupRoutes(Router $router)
    {
        //设置路由命名空间
        $router->group(['namespace' => 'Jai\Contact\Http\Controllers'], function($router)
        {
            require __DIR__.'/Http/routes.php';
        });
    }

    public function register()
    {
        $this->registerContact();
        config([
            'config/contact.php',
        ]);
        
        //这两句是从stripe里摘出来的 绑定名称 以后注入用
        $this->app->singleton('command.cashier.table', function ($app) {
            return new CashierTableCommand;
        });
        //这里相当于注册一个command命令 参数:这里就用到上边的绑定注入了
        $this->commands('command.cashier.table');
    }
    
    private function registerContact()
    {
        $this->app->bind('contact',function($app){
            //这个绑定毫无意义 也许可能是没有用到
            //return new Contact($app);
            return new elick($app);
        });
    }
}
```
<!--more-->
- command 命令
 >这个是把stripe里的CashierTableCommand类直接粘过来.
简单的来说 就是在database\mrigration 创建个迁移文件 然后把内容复制进去 平并且添加这个命令
等安装的时候执行这个命令 并执行 artisan migrate 就安装到数据库里了 并不是自动执行一次性完成的
$this->laravel[]里有很多有用的东西 以后得看看
```
<?php

namespace Laravel\Cashier;

use Illuminate\Console\Command;
use Symfony\Component\Console\Input\InputArgument;

class CashierTableCommand extends Command
{
    /**
     * The console command name.
     *
     * @var string
     */
    protected $name = 'cashier:table';

    /**
     * The console command description.
     *
     * @var string
     */
    protected $description = 'Create a migration for the Cashier database columns';

    /**
     * Execute the console command.
     *
     * @return void
     */
    public function fire()
    {
        //建立迁移文件
        $fullPath = $this->createBaseMigration();
        //填充迁移文件内容
        file_put_contents($fullPath, $this->getMigrationStub());

        $this->info('Migration created successfully!');
        //刷新autoload
        $this->laravel['composer']->dumpAutoloads();
    }

    /**
     * Create a base migration file for the reminders.
     *
     * @return string
     */
    protected function createBaseMigration()
    {
        $name = 'add_cashier_columns';

        $path = $this->laravel['path.database'].'/migrations';

        return $this->laravel['migration.creator']->create($name, $path);
    }

    /**
     * Get the contents of the reminder migration stub.
     *
     * @return string
     */
    protected function getMigrationStub()
    {
        $stub = file_get_contents(__DIR__.'/stubs/migration.stub');

        return str_replace('cashier_table', $this->argument('table'), $stub);
    }

    /**
     * Get the console command arguments.
     *
     * @return array
     */
    protected function getArguments()
    {
        return [
            ['table', InputArgument::REQUIRED, 'The name of your billable table.'],
        ];
    }
}

```

- routes 路由表
>上面已经注册了路由地址和命名空间,所以编写路由分配controller就好了

```
<?php
/**
 * Created by PhpStorm.
 * User: elick
 * Date: 2016/4/1
 * Time: 18:15
 */
//因为使用router->group()这里连命名空间都不用写了
Route::get('contact','ContactController@index');
```
- controller 控制器
>这个也很简单 从这个来看 可以使用原本App的模型.但是如何使用自带模型呢?
~~我现在的想法是和config一样通过publishes 把model直接复制到App\Models下,
也许根本不用,因为现在这个composer已经搜索这个目录 也许直接就可以加载~~
放在目录下可以直接加载 只要继承了 
use Eloquent;
use Illuminate\Database\Eloquent\Model;
这两个都可以 其实Eloquent 相当于快捷方式 引用的也是下面那个类
~~但是还有个问题就是如果有自定义数据库怎么插入数据库 这个一会实验下
安装数据库还没看 找到好的包时看一下~~
通过stripe包 看到他的方法 看上面命令

```
<?php
/**
 * Created by PhpStorm.
 * User: elick
 * Date: 2016/4/1
 * Time: 18:17
 */
namespace Jai\Contact\Http\Controllers;

use App\Http\Controllers\Controller;
use App\Models\Post;
use Illuminate\Support\Facades\Config;
use Jai\Contact\Models\Test;
use View;

class ContactController extends Controller{
    public function index(){
        echo __DIR__.'/../../config';
        echo "<br>";
        echo realpath(__DIR__.'/../../config');
        echo "<br>";
        echo "这就是realpath()的区别";

        $t = Test::all()->toArray();
        dump($t);


        $posts = Post::all()->toArray();
        dump($posts);
        dump(Config::get('contact.message'));
        return view('contact::contact');
        //eturn View::make('contact::contact');
    }
}
```


- model
```
<?php namespace Jai\Contact\Models;

/**
 * Created by PhpStorm.
 * User: elick
 * Date: 2016/4/5
 * Time: 13:55
 */
use Eloquent;
use Illuminate\Database\Eloquent\Model;
class Test extends Model
{

}
```

- contact.blad.php 模版
>其实模版就没啥写的 就是模版文件 上面的服务提供者provider只要写对路径 剩下就是前端页面的事了 很简单

- config 配置文件
>这里的配置文件没写什么
其实关键也就在于如何调用配置文件 使用Config::get()这个方法 然后判断配置内容 根据内容写出逻辑

```
<?php
/**
 * Created by PhpStorm.
 * User: elick
 * Date: 2016/4/1
 * Time: 18:21
 */
return [
    "message" => "Welcome to My new package"
];
```

- 改写composer.json
>这里命名空间一定要搞对,因为服务提供者provider 使用的命名空间是 `Jai\Contact\` 而他的真实路径是 `packages/jai/contact/src` 所以要这么写

```
    "autoload": {
        "classmap": [
            "database"
        ],
        "psr-4": {
            "App\\": "app/",
            "Jai\\Contact\\":"packages/jai/contact/src"
        }
    },
```

- 接下来两个命令

```
//更新Composer的autoloader
composer dump-autoload

//将自定义包的配置文件发布到应用根目录的config目录下以便可以访问。
php artisan vendor:publish
```
