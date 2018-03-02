---
title: Entrust - Laravel 用户权限系统解决方案
date: 2016-06-08 21:48:22
tags: [laravel,php]
categories: php开发
---


## 前言
    最近需要做后台所以找到这个插件.下面的文章内容稍微有点老,但大致讲解的没问题,还是结合项目仓库一起看就明白很多了.
    
## 简单说明
  针对那篇文章没有提到的东西简单说明下
  
- 注意引入文件,升级后很多名称变了,所以一定要参考文档.
<!--more-->
```php
use Zizaco\Entrust\EntrustRole;     //以前引入的文件名不同,以后也要注意

class Role extends EntrustRole
{
}

use Zizaco\Entrust\EntrustPermission;

class Permission extends EntrustPermission
{
}

use Zizaco\Entrust\Traits\EntrustUserTrait;

class User extends Eloquent
{
    use EntrustUserTrait; //这个名称也变了

    ...
}
```

- hasRole和can的用法

```php
//最简单用法
//确认是否在哪个用户组和具有哪个权限
$user->hasRole('owner');   // false
$user->hasRole('admin');   // true
$user->can('edit-user');   // false
$user->can('create-post'); // true

//可一次确认多个用户组和权限
//第三个参数是判断,前面参数是 && 还是 ||
$user->hasRole(['owner', 'admin']);             // true
$user->hasRole(['owner', 'admin'], true);       // false, user does not have admin role
$user->can(['edit-user', 'create-post']);       // true
$user->can(['edit-user', 'create-post'], true); // false, user does not have edit-user permission

//模糊匹配
// match any admin permission
$user->can("admin.*"); // true

// match any permission about users
$user->can("*_users"); // true

//当前登录用户,可以用简单方法快速验证权限
Entrust::hasRole('role-name');
Entrust::can('permission-name');

// is identical to

Auth::user()->hasRole('role-name');
Auth::user()->can('permission-name');

//自定义验证ability()
//三个参数 第一个role 第二个permission 第三个自定义参数  
//这个个没用过暂时这么记录

$options = array(
    'validate_all' => true,
    'return_type' => 'both'
);

list($validate, $allValidations) = $user->ability(
    array('admin', 'owner'),
    array('create-post', 'edit-user'),
    $options
);

var_dump($validate);
// bool(false)

var_dump($allValidations);
// array(4) {
//     ['role'] => bool(true)
//     ['role_2'] => bool(false)
//     ['create-post'] => bool(true)
//     ['edit-user'] => bool(false)
// }

//模版的一些用法
@role('admin')
    <p>This is visible to users with the admin role. Gets translated to 
    \Entrust::role('admin')</p>
@endrole

@permission('manage-admins')
    <p>This is visible to users with the given permissions. Gets translated to 
    \Entrust::can('manage-admins'). The @can directive is already taken by core 
    laravel authorization package, hence the @permission directive instead.</p>
@endpermission

@ability('admin,owner', 'create-post,edit-user')
    <p>This is visible to users with the given abilities. Gets translated to 
    \Entrust::ability('admin,owner', 'create-post,edit-user')</p>
@endability


//剩下还有中间件等等一些用法 看项目文档吧
```


这是我测试时的文件
```php
Route::get('add',function(){
    // Cache为file时 会出现Cache不支持tag的错误
    //换成array就好了
    $admin = new Role;
    $admin->name = 'Admin';
    $admin->save();

    $owner = new Role;
    $owner->name = 'Owner';
    $owner->save();


    $manageUsers = new Permission();
    $manageUsers->name = 'manage_users';
    $manageUsers->display_name = 'Manage Users';
    $manageUsers->save();

    $managePosts = new Permission();
    $managePosts->name = 'manage_posts';
    $managePosts->display_name = 'Manage Posts';
    $managePosts->save();

    //给用户组增加权限
    //从这个版本的文档来看 好像更喜欢用 attachPermission($managePosts) 这种简洁的写法
    $owner->perms()->sync(array($managePosts->id, $manageUsers->id));
    //还可以这么写
    //$admin->attachPermissions($manageUsers,$managePosts);
    
    $admin->perms()->sync(array($managePosts->id));
    //还可以这么写
    //$admin->attachPermission($managePosts);


    // 获取用户
    $user = User::where('name','=','elick')->first();

    // 可以使用 Entrust 提供的便捷方法用户授权
    // 注: 参数可以为 Role 对象, 数组, 或者 ID
        $user->attachRole( $admin );

    // 或者使用 Eloquent 自带的对象关系赋值
        //$user->roles()->attach( $admin->id ); // id only

    $a = $user->hasRole("Owner");    // false
    $b = $user->hasRole("Admin");    // true

    $c = $user->can("manage_posts"); // true
    $d = $user->can("manage_users"); // false

    dump($a);dump($b);dump($c);dump($d);

    //can 和hasRole也能接收数组 一个对全都对
    $user->hasRole(['owner', 'admin']);       // true
    $user->can(['edit-user', 'create-post']); // true

    //如果想要两个参数同为真才行,可以用第三个参数
    $user->hasRole(['owner', 'admin']);             // true
    $user->hasRole(['owner', 'admin'], true);       // false, user does not have admin role
    $user->can(['edit-user', 'create-post']);       // true
    $user->can(['edit-user', 'create-post'], true); // false, user does not have edit-user permission

});

Route::get('test',function(){
    $user = User::where('name','=','elick')->first();
    $a = $user->ability(['Admin','Owner'], ['manage_posts','manage_users']);
    dump($a);

});

```


## 参考文件

[项目地址:https://github.com/Zizaco/entrust](https://github.com/Zizaco/entrust)
[Entrust - Laravel 用户权限系统解决方案](https://phphub.org/topics/166)
