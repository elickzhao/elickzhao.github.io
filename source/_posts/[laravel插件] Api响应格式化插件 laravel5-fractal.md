---
title: (laravel插件) Api响应格式化插件 laravel5-fractal
tags: laravel
categories: 前端技术
abbrlink: 32860
date: 2016-10-21 22:31:25
---

## 简介
这个插件用到的是 **Fractal**,并把它与 **laravel** 结合.使 **larvel** 响应请求时有美化的格式,不用每个都去手动改写.
例如,数据库里的表名往往是简称或英文.但是你返回给程序时,最好直接就能用,而不用再一次进行操作,并且可以添加没有记录在数据库里的数据.

## 例子
```php
//从数据里读出的数据
$books = [
	[
		'id' => '1',
		'title' => 'Hogfather',
		'yr' => '1998',
		'author_name' => 'Philip K Dick',
		'author_email' => 'philip@example.org',
	],
	[
		'id' => '2',
		'title' => 'Game Of Kill Everyone',
		'yr' => '2014',
		'author_name' => 'George R. R. Satan',
		'author_email' => 'george@example.org',
	]
];

<!--more-->

//模版
public function transform($books)
{
    return [
        'id'      => (int) $book['id'],
        'title'   => $book['title'],
        'year'    => (int) $book['yr'],     // 把数据库不标准的命名改成可读性强的
        'author'  => [                      // 整理在author下 数据库是没有这个层级关系的
        	'name'  => $book['author_name'],
        	'email' => $book['author_email'],
        ],
        'links'   => [                  // 连接这种东西就完全没必要存在数据库里 直接组合就成了
            [
                'rel' => 'self',
                'uri' => '/books/'.$book['id'],
            ]
        ]
    ];
}

// 这样返回就好了 就是有一定格式的数据了 省着写程序了
return Fractal::collection($users, new UserTransformer);

```

## 使用

- 首先去建立个模版,使用命令 `php artisan make:transformer` 这个命令会建立一个目录 `App\Transfomers\ ` 把所有的模版都放在下面
- 紧接着修改模版到你想输出的样式
```php
public function transform($user)
{
    return [
           'id' => $user->user_id,
           'name' => "{$user->user_firstname} {$user->user_lastname}",
           ...
           ];
}
```

- 2.0版 你可以直接用这个命令 生成对应model的模版 `php artisan make:transformer UserTransformer -m User`
- 填写数据使用模版
```php
// 单个数据使用 item
$user = User::find(1);

return Fractal::item($user, new UserTransformer);

// 多个数据使用 collection
$users = User::get(); // $users = User::paginate();

return Fractal::collection($users, new UserTransformer);

// 如果想用数组这么写
Fractal::collection($user, new UserTransformer)->getArray();

```

## 参考文档
[Cyvelnet/laravel5-fractal](https://github.com/Cyvelnet/laravel5-fractal)
[Fractal](http://fractal.thephpleague.com/simple-example/)
