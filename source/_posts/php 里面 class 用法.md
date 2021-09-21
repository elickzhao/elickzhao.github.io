---
title: php 里面 class 用法
tags: php
abbrlink: 56184
date: 2016-10-26 22:31:24
categories:
---

>自 PHP 5.5 起，关键词 class 也可用于类名的解析。使用 ClassName::class 你可以获取一个字符串，包含了类 ClassName 的完全限定名称。这对使用了 命名空间 的类尤其有用。

```php
namespace App\Http\Controllers\Api\V1;

use ApiDemo\Repositories\Contracts\UserRepositoryContract;


## 当使用命名空间时 得到的结果是类文件所在位置  ??不过这个位置现在是不对的 可能我用法错误 

var_dump(ApiDemo\Repositories\Contracts\UserRepositoryContract::class);
//"App\Http\Controllers\Api\V1\ApiDemo\Repositories\Contracts\UserRepositoryContract"


## 当使用命名空间时  得到的是命名空间

var_dump(UserRepositoryContract::class);
// "ApiDemo\Repositories\Contracts\UserRepositoryContract"
```
