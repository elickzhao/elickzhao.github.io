---
title: laravel 数据库查询优化
date: 2016-12-01 23:28:44
tags: [laravel,php]
categories:
---


```php
// 使用 Cache::remember 缓存结果是个不错的选择,能大大优化访问速度
    $cid = '分类ID';
    $cat = Cache::remember('cat-' . $cid, Carbon::now()->addMinutes(60), function () use ($cid) {
          return Category::where([['parent_id', $cid], ['is_show', 1]])
          ->select('cat_id', 'cat_name', 'parent_id', 'style')
          ->orderBy('sort_order')
          ->get();
        });


    $allCat = Cache::remember('allCat', Carbon::now()->addHour(3), function () {
            return Category::where('is_show', 1)
            ->select('cat_id', 'cat_name', 'parent_id')
            ->orderBy('sort_order')
            ->get()
            ->toArray();
        });
```


[Laravel 5 性能优化技巧](http://www.cnblogs.com/wish123/p/5503870.html)
[使用 OpCache 提升 PHP 5.5+ 程序性能](https://laravel-china.org/topics/301)
[大数据量高并发的数据库优化](http://www.ithao123.cn/content-960653.html)
[使用 Laravel 的服务容器来优化读写数据库中的 options](http://www.tuicool.com/articles/bIVBFj)
[Laravel大量数据库查询导致php进程内存耗尽](http://blog.csdn.net/geniuslinchao/article/details/51491308)
