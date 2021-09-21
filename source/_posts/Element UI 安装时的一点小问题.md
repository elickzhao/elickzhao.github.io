---
title: Element UI 安装时的一点小问题
tags:
  - JavaScript
  - 小百科
categories: 前端技术
abbrlink: 37919
date: 2016-10-19 22:34:16
---

Element 用到了 `vue-markdown-loader` 使 md 文件生成网页. 但是他在 `webpace` 启动时 把生成的网页缓存到了. `vue-markdown-loader` 目录下的 `.cache` 目录里. 但如果是windows系统的话.有可能因为权限问题无法创建 `.`开头文件造成错误. 所以 自己建立一个就好了
