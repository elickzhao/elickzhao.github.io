---
title: 'SyntaxError-> Unexpected token "{" 注意chrome插件'
tags:
  - js
  - 前端
  - 小百科
abbrlink: 58166
date: 2018-02-03 20:02:20
categories:
---

今天遇到个奇葩问题,有个`js`报错 结果出现位置居然在 `<!DOCTYPE HTML>` 这里.

真是凌乱了.找了半天,把整个页面都删除了,还是报错了. 感觉可能并不是自己的问题.

拿出火狐再看页面就没有错了. 后来发现居然是chrome的错 而且莫名的今天就坏了 前两天都没问题啊

可能他们这个插件更新 然后浏览器 自己就更新吧 总之就是插件的问题 

把文件名扔到百度 根本查不出是啥 幸好眼尖发现了 logo  一看知道是啥插件  删掉后问题解决

其实就是没经验 英语差 `chrome-extension` 一看就应该知道是 插件的问题

`lcbgjpbhnnjajgjlpdemhadfceacidme` 这就是那个插件名 就是该死的 夜间模式

当然这次是这个出问题了 下次有可能是别的 因为在论坛里看到 有人说 有道词典 有这个问题.

所以注意下 以后遇到就看看是那个插件吧 

```js
Error in event handler for (unknown): SyntaxError: Unexpected token {
    at eval (eval at Updater.check (chrome-extension://lcbgjpbhnnjajgjlpdemhadfceacidme/js/gmWrapper.js:4:3244), <anonymous>:1:327)
    at Updater.check (chrome-extension://lcbgjpbhnnjajgjlpdemhadfceacidme/js/gmWrapper.js:4:3244)
    at init (chrome-extension://lcbgjpbhnnjajgjlpdemhadfceacidme/js/jquery-1.7.2.js:1:3311)
    at onReadyGM (chrome-extension://lcbgjpbhnnjajgjlpdemhadfceacidme/js/jquery-1.7.2.js:1:76)
    at Object.onInitializedGM (chrome-extension://lcbgjpbhnnjajgjlpdemhadfceacidme/js/gmWrapper.js:4:636)
    at Object.callbackResponse (chrome-extension://lcbgjpbhnnjajgjlpdemhadfceacidme/js/gmWrapper.js:4:331)
    at chrome-extension://lcbgjpbhnnjajgjlpdemhadfceacidme/js/gmWrapper.js:4:3378
```