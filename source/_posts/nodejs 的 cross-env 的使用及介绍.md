---
title: nodejs 的 cross-env 的使用及介绍
date: 2017-06-21 23:11:58
tags: [node.js,前端]
categories:
---

这个小插件主要解决 windows 下 无法设置 `NODE_ENV=development` 这个东西. 因为程序可以根据 NODE的环境变量,做出不同选择.

这个在 **linux** 下是可以设置的,但在 **windows** 下不行

**安装**
```
npm install across-env --save-dev
```

**使用**
在package.json里,需要设置环境的地方加上这句就好了
```
 cross-env NODE_ENV=development
```


## 参考文档
[使用cross-env解决跨平台设置NODE_ENV的问题](https://segmentfault.com/a/1190000005811347)
