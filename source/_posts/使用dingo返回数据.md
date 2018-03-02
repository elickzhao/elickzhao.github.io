---
title: 使用dingo返回数据
date: 2016-12-19 23:22:35
tags: [laravel,dingo]
categories:
---

## 简介
使用dingo返回数据,一开始有点懵,现在开始有点明晰了.


## 详细

**方法一**
用Dingo的Transformer. 其实这两种写法都可以 用Transformer()更简洁和灵活一些.
因为Transformer()不仅可以利用 `return $ECUser->attributesToArray();` 将属性转换成数组
还可以添加和转换数据库中没有的字段,还可以用 `include` 做类似关联表的作用,具体看这里: [Dingo Transformers 的使用(Fractal)](https://www.zybuluo.com/mdeditor#553839)
所以这个很灵活,可以在这个类里添加更多的操作,当然只是返回array的话,还是稍显麻烦因为还得建立个文件.

**补充:**
Dingo返回数据几种形式:

Responding With An Array 响应一个数组
`return $this->response->array($user->toArray());`

Responding With A Single Item 响应一个元素
`return $this->response->item($user, new UserTransformer);`

Responding With A Collection Of Items 响应一个元素集合
`return $this->response->collection($users, new UserTransformer);`

Responding With Paginated Items 分页响应
`return $this->response->paginator($users, new UserTransformer);`

[更多看这里........](https://github.com/liyu001989/dingo-api-wiki-zh/blob/master/Responses.md)

<!--more-->

```php

    $cat = Category::where([['parent_id', $cid], ['is_show', 1]])->select('cat_id', 'cat_name', 'parent_id', 'style')->orderBy('sort_order')->get();
    //用dingo的response就是省了加json头这个步骤了
    //看来不仅如此啊 加上返回的是一个数组 data[{取出的数据}] 如果不用就返回{取出的数据}
    return $this->response->collection($cat, new CategoryTransformer());

```

**方法二**
直接用array开发是更快一些,因为不用去建立文件.
这里是利用的Dingo的reponse并不是laravel的,所以要看清

```php
    // 这里返回一定要是toArray()
    $wallet = ECUser::where('user_id',$id)->select('user_money','pay_points')->first()->toArray();
    return $this->response->array($wallet);
```

**方法三**
完全用laravel来返回 json, 不用插件来做.

```php
    $wallet = ECUser::where('user_id',$id)->select('user_money','pay_points')->first()->toArray();

    //上面那个用的是dingo的response和下面这个不一样,下面的是laravel的
    //这两个是有区别的 dingo的只有array()没有json, laravel的正好相反,有json()没有array() 但他们的结果是一样的
    return response()->json($wallet);
```