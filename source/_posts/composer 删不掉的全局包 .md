---
title: composer 删不掉的全局包
tags: composer
abbrlink: 58242
date: 2017-12-18 22:18:40
categories:
---

今天安装 `php-cs-fixer` 遇到个讨厌的情况.一直提示我需要卸载 `symfony/console`

```
Problem 1
    - Installation request for friendsofphp/php-cs-fixer ^2.9 -> satisfiable by friendsofphp/php-cs-fixer[v2.9.0].
    - Conclusion: remove symfony/console v2.8.32
    - Conclusion: don't install symfony/console v2.8.32
    - friendsofphp/php-cs-fixer v2.9.0 requires symfony/console ^3.2 || ^4.0 -> satisfiable by symfony/console[v3.2.0, v3.2.1, v3.2.10, v3.2.11, v3.2.12, v3.2.13, v3.2.14, v3.2.2, v3.2.3, v3.2.4, v3.2.5, v3.2.6, v3.2.7, v3.2.8, v3.2.9, v3.3.0, v3.3.1, v3.3.10, v3.3.11, v3.3.12, v3.3.13, v3.3.14, v3.3.2, v3.3.3, v3.3.4, v3.3.5, v3.3.6, v3.3.7, v3.3.8, v3.3.9, v3.4.0, v3.4.1, v3.4.2, v4.0.0, v4.0.1, v4.0.2].
```

可是 `composer remove symfony/console -g` 却提示说根本没有安装 
而且在`C:\Users\elick\AppData\Roaming\Composer`的 `composer.json` 也确实没有啊 这就晕了

最后无奈更新了下 `composer update` 结果在更新的信息里的确发现了 `symfony/console`

后来才终于找到 原来在这里安装着呢 `C:\Users\elick\AppData\Roaming\Composer\vendor\composer`的 `installed.json` 

这可能是`composer`自带的插件或者依赖的插件吧 可能我用了好久一直没更新 于是出现了这个错误 

看来以后安装插件的时候,提前更新下 是个好操作啊.

