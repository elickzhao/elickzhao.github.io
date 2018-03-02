---
title: THINKPHP 清空数据缓存方法
date: 2018-01-26 22:37:48
tags: [php,小百科]
categories:
---

```php
$cache = new \Think\Cache;
//第一个参数为 缓存类型 这里是从配置里读取  第二个参数为清理的文件夹 因为默认清理的是 Temp 这里修改成Cache
echo $cache->getInstance(C('DATA_CACHE_TYPE'), ['temp'=>CACHE_PATH])->clear();
//如果是清理Temp 直接不用参数就可以用了 不用这么麻烦
//$cache->getInstance()->clear();
```

## 参考文档
[THINKPHP 清空数据缓存方法](http://blog.csdn.net/chenzhuyu/article/details/51713966)