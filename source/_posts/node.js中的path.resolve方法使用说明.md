---
title: node.js中的path.resolve方法使用说明
tags:
  - js
  - node.js
abbrlink: 33891
date: 2017-01-10 19:57:01
categories:
---


方法说明：
将参数 to 位置的字符解析到一个绝对路径里。
```
path.resolve([from ...], to)
```

## 简介
当使用插件不在标准路径的时候,可以使用这个函数方便的引入.

---

**由于该方法属于path模块，使用前需要引入path模块（var path= require(“path”) ）**
**接收参数：**
from                     源路径
to                         将被解析到绝对路径的字符串

**例子：**

```js
path.resolve('/foo/bar', './baz')
 
// returns
 
'/foo/bar/baz'
 
path.resolve('/foo/bar', '/tmp/file/')
 
// returns
 
'/tmp/file'
 
path.resolve('wwwroot', 'static_files/png/', '../gif/image.gif')
 
// if currently in /home/myself/node, it returns
 
'/home/myself/node/wwwroot/static_files/gif/image.gif'
```
[node.js中的path.resolve方法使用说明](http://www.jb51.net/article/58295.htm)

