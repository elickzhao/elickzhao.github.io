---
title: vue + webpack 学习简介
date: 2016-12-14 23:34:24
tags: [js,vue,webpack]
categories:
---

## 一些经验


**自动刷新问题:**
用 **vue-cli** 建立的vue项目,配合 webpack 测试时, webpack 可以自动更新浏览器输出结果,但是 vue.js 的数据修改,是不会显示在浏览器中的,必须手动刷新才行.
比如下面例子
```html
<template>
  <div class="hello">
    <h1>{{ msg }}  hi 你好</h1> <!-- 在这里修改就会立马显示  -->
  </div>
</template>

<script>
export default {
  data () {
    return {
      // note: changing this line won't causes changes
      // with hot-reload because the reloaded component
      // preserves its current state and we are modifying
      // its initial state.
      msg: 'Hello World! hi 你好' //修改这里浏览器不会自动刷新
    }
  }
}
</script>
```


[Vue.js + webpack 项目实践](http://www.open-open.com/lib/view/open1435200052247.html)
[Vue.js——60分钟webpack项目模板快速入门](http://www.w2bc.com/article/159036)
[一小时包教会 —— webpack 入门指南](http://www.w2bc.com/Article/50764)
