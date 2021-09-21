---
title: php 抽象类的用法 (abstract)
tags: php
abbrlink: 4132
date: 2016-10-25 22:31:25
categories:
---


抽象类(abstract),一般用于基类,所有的公共方法都可以放在里面,抽象类是不能被实例化的,只能被继承.而且抽象方法是必须被重写的,其他可以根据需要选择.这样你可以简单只写一个抽象方法,其他方法大家都有了,省着写了.

**以下是官方说法**

>PHP 5 支持抽象类和抽象方法。定义为抽象的类不能被实例化。任何一个类，如果它里面至少有一个方法是被声明为抽象的，那么这个类就必须被声明为抽象的。被定义为抽象的方法只是声明了其调用方式（参数），不能定义其具体的功能实现。

<!--more-->

**例子:**
抽象类 `BaseRepository`
```php
<?php

namespace ApiDemo\Repositories\Eloquent;

abstract class BaseRepository
{
    protected $model;

    public function __construct()
    {
        $this->model = app()->make($this->model());
    }

    abstract public function model();

    public function paginate($limit = null)
    {
        return $this->model
            ->paginate($limit);
    }

    public function where(array $data)
    {
        return $this->model->where($data);
    }

    public function first()
    {
        return $this->model->first();
    }

    public function find($id)
    {
        return $this->model->find($id);
    }

    public function create(array $attributes)
    {
        $model = $this->model->newInstance($attributes);
        $model->save();

        return $model;
    }

    public function update($id, array $attributes)
    {
        // 感觉不太对
        $model = $this->model->find($id);
        $model->fill($attributes)->save();

        return $model;
    }

    public function destroy($id)
    {
        return $this->model->destroy($id);
    }
}

```

集成类 UserRepository
```php
<?php

namespace ApiDemo\Repositories\Eloquent;

use ApiDemo\Repositories\Contracts\UserRepositoryContract;

class UserRepository extends BaseRepository implements UserRepositoryContract
{
    public function model()
    {
        return 'ApiDemo\Models\User';
    }
}

```

从上面可以看到,只要实现抽象方法 `model()` 就可以了.这样指定了表,一些相同查询方法,大家都可以用了.

## 参考文档
[官方网站](http://php.net/manual/zh/language.oop5.abstract.php)
