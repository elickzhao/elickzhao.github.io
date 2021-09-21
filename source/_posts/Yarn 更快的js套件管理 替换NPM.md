---
title: Yarn 更快的js套件管理 替换NPM
tags:
  - js
  - npm
  - node.js
abbrlink: 63668
date: 2017-01-31 22:03:51
categories:
---

>新一代戰神 Yarn 終於在昨天出爐了，Yarn 跟 Npm 一樣都是 JavaScript 套件版本管理工具，但是 Npm 令人詬病的是安裝都是非常的慢，快取機制用起來效果也不是很好，所以 Yarn 的出現解決了這些問題，透過 Yarn 安裝過的套件都會在家目錄產生 Cache (目錄在 ~/.yarn-cache/)，也就是只要安裝過一次，下次砍掉 node_modules 目錄重新安裝都會從 Cache 讀取。

## 安装
1. 如果也就安装了npm 那么很简单 `npm install -g yarn `
2. ~~如果没有安装npm,而且不想用npm了,那只有去官网下载安装包.下载地址：[https://yarnpkg.com/latest.msi](https://yarnpkg.com/latest.msi) 不过被墙的厉害,所以需要有些准备.~~ 现在官网可以访问了,而且速度很快并且有中文,搜索包也快很多. [https://yarnpkg.com/zh-Hans/](https://yarnpkg.com/zh-Hans/)
3. 以前说那个下载的安装程序,这次尝试了,不过安装特别讨厌,并没有自动把命令放到环境变量里.而且安装位置也不好.所以还是用 npm 安装吧 比较简单
4. 在强调一些搜索包真是太方便了,全是中文[https://yarnpkg.com/zh-Hans/packages](https://yarnpkg.com/zh-Hans/packages)


##yarn更换下载源

// 查看下载源

    yarn config get registry

// 更换为淘宝源

    yarn config set registry https

虽然原来的源还不错,不过下载些被墙了的和大的插件还是比较慢,所以换了淘宝.
不过淘宝这个源不太稳定,还是怎么回事.有时说插件都安装成功了,不过使用的时候就报错.
npm 就不会 搞不懂啊.

## 命令行对比
[官网原文链接](https://yarnpkg.com/en/docs/migrating-from-npm)
|npm |	Yarn |
| --------   | -----  | ----  |
|npm install |	yarn install |
|(N/A)	|yarn install --flat|
|(N/A)	|yarn install --har|
|(N/A)	|yarn install --no-lockfile|
|(N/A)	|yarn install --pure-lockfile|
|npm install [package]	|(N/A)|
|npm install --save [package]	|yarn add [package]|
|npm install --save-dev [package]	|yarn add [package] [--dev/-D]|
|(N/A)	|yarn add [package] [--peer/-P]|
|npm install --save-optional [package]	|yarn add [package] [--optional/-O]|
|npm install --save-exact [package]	|yarn add [package] [--exact/-E]|
|(N/A)	|yarn add [package] [--tilde/-T]|
|npm install --global [package]	|yarn global add [package]|
|npm rebuild	|yarn install --force|
|npm uninstall [package]	|(N/A)|
|npm uninstall --save [package]	|yarn remove [package]|
|npm uninstall --save-dev [package]	|yarn remove [package]|
|npm uninstall --save-optional [package]	|yarn remove [package]|
|npm cache clean	|yarn cache clean|
|rm -rf node_modules && npm install	|yarn upgrade|


## 参考文档
[Yarn的安装与使用详细介绍](http://www.jb51.net/article/95630.htm)