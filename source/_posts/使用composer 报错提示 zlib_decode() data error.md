---
title: 使用composer 报错提示 Failed to decode response zlib_decode() data error
tags:
  - composer
  - 小百科
abbrlink: 14095
date: 2016-10-24 22:37:31
categories:
---



```
Failed to decode response: zlib_decode(): data error
Retrying with degraded mode, check https://getcomposer.org/doc/articles/troubleshooting.md#degraded-mode for more info
```

网上有人说这个只要使用 `composer selfupdate` 就可以解决. 其实并不完全对.
因为我是再使用 `composer selfupdate` 这个时提示这个错误的.
其原因是因为权限不足造成的.我再 **windows** 下并没有使用 **cmd** 而是使用自己下载的 **Cmder**.
所以会有权限的问题.同理 **linux** 下如果权限不足也会出现这个错误.



