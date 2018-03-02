---
title: webpack的'Cannot find module 'webpacklibnodeNodeTemplatePlugin'问题
date: 2017-01-11 20:55:10
tags: [js,webpack,node.js,小百科]
categories:
---

- 第一步：`npm config get prefix` ，获取输出path `C:\Users\jaxGu\AppData\Roaming\npm`加上`\node_modules`用于第二步值

 

- 第二步：添加系统环境变量：`NODE_PATH:C:\Users\jaxGu\AppData\Roaming\npm\node_modules`

 
- 第三步：关掉命令行，重新打开。

**参考文档**
[『奇葩问题集锦』Cannot find module 'webpack/lib/node/NodeTemplatePlugin'](http://www.cnblogs.com/guxuelong/p/5288390.html)
[webpack的问题](http://www.bubuko.com/infodetail-1420423.html)


