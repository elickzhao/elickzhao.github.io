---
title:  laravel 一些经验
date: 2016-12-9 21:06:18
tags: [php,laravel]
categories:
---

Route::auth(); 5.2新加的用于生成auth路由

Auth::guard($guard) 试图从本地缓存中获取保护 官方直译是这么写的 但还是不明白啥意思 而且这也是5.2新加的
有点懂这个guard了 主要用这个机制应对同一应用不同登录验证 比如前台用户登录和后台管理员登录 就可以放在两个表里然后配置不同的guard

5.2和5.1的Auth部分变化很大,两个config/auth.php配置就有很大区别

5.2把单独Auth/Guard 类 拆成了三个,SessionGuard和TokenGuard,GuardHelpers.
GuardHelpers主要是共有方法,另外两个是根据不同驱动,也就是登录信息保存方法.
同时也就是根据pc和移动的登录验证分成了两个类.


---

    laravel是区分大小写的,即使在windows下如果url路径写错(大小写不一致),也会造成找不到页面错误

----

TaskRepository 分担查询数据库内容的工作 其实放到model也可以 不过这样解耦 以后方便改吧

```php
RouteServiceProvider --> $router->model('task','App\Task'); #路由绑定模型 还得需要前边 路由的配合
Route::delete('tasks/{task}','TaskController@destroy');  #再说明一次 task是指第一个参数 也就是起得名字

AuthServiceProvider -->     protected $policies = [
        'App\Model' => 'App\Policies\ModelPolicy',
        'App\Task' => 'App\Policies\TaskPolicy',
    ];
```

关联策略的 策略是用于检查是否有权限进行这个操作
```php
    public function destroy(User $user, Task $task)
    {
        return $user->id === $task->user_id;
    }
```
```
$this->authorize('destroy',$task);
```
调用策略验证  同名功能 其实可以不用第一个参数
> 默认策略必须在AuthServiceProvider这个支持者里进行设定才能应用
```
<?php

namespace App\Providers;

use App\Post;
use App\Policies\PostPolicy;
use Illuminate\Contracts\Auth\Access\Gate as GateContract;
use Illuminate\Foundation\Support\Providers\AuthServiceProvider as ServiceProvider;

class AuthServiceProvider extends ServiceProvider
{
    /**
     * The policy mappings for the application.
     *应用策略映射
     * @var array
     */
    protected $policies = [
        'App\Model' => 'App\Policies\ModelPolicy',
        //放在这里直接映射了 下面就不用在boot里挨个写了 那样太凌乱了
        Post::class=>PostPolicy::class,
    ];

    /**
     * Register any application authentication / authorization services.
     *
     * @param  \Illuminate\Contracts\Auth\Access\Gate  $gate
     * @return void
     */
    public function boot(GateContract $gate)
    {
        parent::registerPolicies($gate);

        //定义权限
        $gate->define('update-post',function($user,$post){
           return $user->id === $post->user_id;
        });
        //也可以这么写
        //$gate->define('update-post', 'PostPolicy@update');
    }
```
<!--more-->
-----

**如果要指定一个表seed迁移的话命令这么写**
如果是新添加的表增加内容 一定要用这个--class 要不有可能无法添加进去内容
`artisan db:seed --class=RoleTableSeeder`
一定要加 --class 要不无法添加的数据库内

工厂模式也很简单 只要在工厂文件里ModelFactory不断定义要迁移的表就可以了

然后在seed文件里就可以使用factory(App\Models\Role::class,10)->create();了


**Fnaker常用表**
[详细地址](https://github.com/fzaninotto/Faker)
```
    'title' => $faker->text(5),         //最大为5的随机字母
    'content' => $faker->realText(),    //默认为200的完整意义的话
    'desc' => $faker->realText(10),     //10就是10个字符了 很难有完整意义了
    'user_id' => $faker->numberBetween(1,4),    //限定最小最大值的随机数字
    'status' => $faker->numberBetween(0,1)
    'name' => $faker->country           //国家名
    'name' => $faker->name,             //人名
    'email' => $faker->email,           //电子邮件
    'content' => 'comment '.$faker->randomDigitNotNull, //个位数 没有空 随机
    'item_type' => 'App\Model\\'.$faker->randomElement(['Post','Video']) //限定元素 随机
```
