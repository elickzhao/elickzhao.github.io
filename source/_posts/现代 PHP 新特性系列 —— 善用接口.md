---
title: 现代 PHP 新特性系列 —— 善用接口
date: 2016-05-26 18:24:15
tags: [php,laravel]
categories: php开发
---

# 前言
接口一直让我很晕,下面这篇文章有了很好的说明.
[现代 PHP 新特性系列（二） —— 善用接口](http://laravelacademy.org/post/4246.html)
这里我就再简单化下,以便我能快速记忆.

------

# 简要说明
## 接口说直观点就是模具,或者说API

- API说
说他像API,是因为他提出来所有功能.你知道接口有什么,你就知道你能做什么了

- 模具说
说他像模具,是因为接口的功能是统一的,一致的.但是实现的方法可以不同.就像你做一个花瓶的模具,但是根据填充的材料不同,可以做出不同的花瓶.什么玻璃的,陶瓷的,珐琅的.虽然他们长的都一个样.

## 接口的作用
所以接口的作用,就是解耦.

比如一个DB类流程:

- DB操作类,根据DB接口提供的功能,进行具体功能开发.
- DB实例化的时候,必须传入符合DB接口的DB驱动类.(符合接口保证功能可用,而且不符合接口也会报错)
- DB操作类,根据传入不同DB驱动实例,达到解耦的作用.(因为功能名称都是统一的)

<!--more-->

**大致如下图:**
![接口流程图](/image/16-5/interface.svg)


**代码参考 用的不是DB的例子**
**Store类**
```php
<?php

namespace Illuminate\Contracts\Cache;

interface Store
{
    /**
     * Retrieve an item from the cache by key.
     *
     * @param  string|array  $key
     * @return mixed
     */
    public function get($key);

    /**
     * Retrieve multiple items from the cache by key.
     *
     * Items not found in the cache will have a null value.
     *
     * @param  array  $keys
     * @return array
     */
    public function many(array $keys);

    /**
     * Store an item in the cache for a given number of minutes.
     *
     * @param  string  $key
     * @param  mixed   $value
     * @param  int     $minutes
     * @return void
     */
    public function put($key, $value, $minutes);

    /**
     * Store multiple items in the cache for a given number of minutes.
     *
     * @param  array  $values
     * @param  int  $minutes
     * @return void
     */
    public function putMany(array $values, $minutes);

    /**
     * Increment the value of an item in the cache.
     *
     * @param  string  $key
     * @param  mixed   $value
     * @return int|bool
     */
    public function increment($key, $value = 1);

    /**
     * Decrement the value of an item in the cache.
     *
     * @param  string  $key
     * @param  mixed   $value
     * @return int|bool
     */
    public function decrement($key, $value = 1);

    /**
     * Store an item in the cache indefinitely.
     *
     * @param  string  $key
     * @param  mixed   $value
     * @return void
     */
    public function forever($key, $value);

    /**
     * Remove an item from the cache.
     *
     * @param  string  $key
     * @return bool
     */
    public function forget($key);

    /**
     * Remove all items from the cache.
     *
     * @return void
     */
    public function flush();

    /**
     * Get the cache key prefix.
     *
     * @return string
     */
    public function getPrefix();
}
```


----------
**MemcachedStore类**
```php
<?php

namespace Illuminate\Cache;

use Memcached;
use Illuminate\Contracts\Cache\Store;

class MemcachedStore extends TaggableStore implements Store
{
    /**
     * The Memcached instance.
     *
     * @var \Memcached
     */
    protected $memcached;

    /**
     * A string that should be prepended to keys.
     *
     * @var string
     */
    protected $prefix;

    /**
     * Create a new Memcached store.
     *
     * @param  \Memcached  $memcached
     * @param  string      $prefix
     * @return void
     */
    public function __construct($memcached, $prefix = '')
    {
        $this->setPrefix($prefix);
        $this->memcached = $memcached;
    }

    /**
     * Retrieve an item from the cache by key.
     *
     * @param  string|array  $key
     * @return mixed
     */
    public function get($key)
    {
        $value = $this->memcached->get($this->prefix.$key);

        if ($this->memcached->getResultCode() == 0) {
            return $value;
        }
    }

    /**
     * Retrieve multiple items from the cache by key.
     *
     * Items not found in the cache will have a null value.
     *
     * @param  array  $keys
     * @return array
     */
    public function many(array $keys)
    {
        $prefixedKeys = array_map(function ($key) {
            return $this->prefix.$key;
        }, $keys);

        $values = $this->memcached->getMulti($prefixedKeys, null, Memcached::GET_PRESERVE_ORDER);

        if ($this->memcached->getResultCode() != 0) {
            return array_fill_keys($keys, null);
        }

        return array_combine($keys, $values);
    }

    /**
     * Store an item in the cache for a given number of minutes.
     *
     * @param  string  $key
     * @param  mixed   $value
     * @param  int     $minutes
     * @return void
     */
    public function put($key, $value, $minutes)
    {
        $this->memcached->set($this->prefix.$key, $value, $minutes * 60);
    }

    /**
     * Store multiple items in the cache for a given number of minutes.
     *
     * @param  array  $values
     * @param  int  $minutes
     * @return void
     */
    public function putMany(array $values, $minutes)
    {
        $prefixedValues = [];

        foreach ($values as $key => $value) {
           $prefixedValues[$this->prefix.$key] = $value;
        }

        $this->memcached->setMulti($prefixedValues, $minutes * 60);
    }

    /**
     * Store an item in the cache if the key doesn't exist.
     *
     * @param  string  $key
     * @param  mixed   $value
     * @param  int     $minutes
     * @return bool
     */
    public function add($key, $value, $minutes)
    {
        return $this->memcached->add($this->prefix.$key, $value, $minutes * 60);
    }

    /**
     * Increment the value of an item in the cache.
     *
     * @param  string  $key
     * @param  mixed   $value
     * @return int|bool
     */
    public function increment($key, $value = 1)
    {
        return $this->memcached->increment($this->prefix.$key, $value);
    }

    /**
     * Decrement the value of an item in the cache.
     *
     * @param  string  $key
     * @param  mixed   $value
     * @return int|bool
     */
    public function decrement($key, $value = 1)
    {
        return $this->memcached->decrement($this->prefix.$key, $value);
    }

    /**
     * Store an item in the cache indefinitely.
     *
     * @param  string  $key
     * @param  mixed   $value
     * @return void
     */
    public function forever($key, $value)
    {
        $this->put($key, $value, 0);
    }

    /**
     * Remove an item from the cache.
     *
     * @param  string  $key
     * @return bool
     */
    public function forget($key)
    {
        return $this->memcached->delete($this->prefix.$key);
    }

    /**
     * Remove all items from the cache.
     *
     * @return void
     */
    public function flush()
    {
        $this->memcached->flush();
    }

    /**
     * Get the underlying Memcached connection.
     *
     * @return \Memcached
     */
    public function getMemcached()
    {
        return $this->memcached;
    }

    /**
     * Get the cache key prefix.
     *
     * @return string
     */
    public function getPrefix()
    {
        return $this->prefix;
    }

    /**
     * Set the cache key prefix.
     *
     * @param  string  $prefix
     * @return void
     */
    public function setPrefix($prefix)
    {
        $this->prefix = ! empty($prefix) ? $prefix.':' : '';
    }
}
```

----------
**CacheStore类**
```php
<?php
namespace App\Tests;

use Illuminate\Contracts\Cache\Store;

class CacheStore
{
    protected $store;

    public function __construct(Store $store)
    {
        $this->store = $store;
    }

    public function get($key)
    {
        return $this->store->get($key);
    }

    public function put($key, $value, $minutes=1)  
    {
        $this->store->put($key, $value, $minutes);
    }

    public function forget($key)
    {
        $this->store->forever($key);
    }

    public function flush()
    {
        $this->store->flush();
    }
}
```
**使用CacheStore类**
```php
$memcached = new \Memcached();
$memcached->addServer('localhost',11211);
$memcachedCache = new MemcachedStore($memcached);
$cacheStore = new CacheStore($memcachedCache);
$cacheStore->put('site','http://LaravelAcademy.org');
dd($cacheStore->get('site'));
```