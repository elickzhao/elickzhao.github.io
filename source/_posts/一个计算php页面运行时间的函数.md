---
title: 一个计算php页面运行时间的函数
tags:
  - php
  - 小百科
abbrlink: 43233
date: 2016-12-02 22:46:40
categories:
---

代码片段

```php
/*
@ 计算php程序运行时间
*/
function microtime_float()
{
list($usec, $sec) = explode(” “, microtime());
return ((float)$usec + (float)$sec);
}
//开始计时，放在头部
$starttime = microtime_float();
//结束计时，放在最底部
$runtime = number_format((microtime_float() – $starttime), 4).'s';
//输出
echo ‘RunTime:'.$runtime;
```
