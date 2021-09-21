---
title: css文件报错Uncaught SyntaxError Unexpected token
tags:
  - css
  - 前端
abbrlink: 61701
date: 2018-01-31 18:47:50
categories:
---

**有时图省事 直接复制改写就会出现这个问题 所以要注意**

错误原因：
错误代码<script src="${ctxStatic}/app/common/css/dropload.css"></script>
改成
<link rel="stylesheet" href="${ctxStatic}/app/common/css/dropload.css">
复制粘贴害人不浅啊T_T

## 参考文档
[css文件报错Uncaught SyntaxError: Unexpected token .](http://blog.csdn.net/sunranword/article/details/78892562)