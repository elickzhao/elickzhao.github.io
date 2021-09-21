---
title: bootstrap 该死的IE兼容
tags:
  - bootstrap
  - 前端
  - 浏览器兼容
categories: 前端技术
abbrlink: 64778
date: 2016-05-17 21:18:46
---

# 前面碎碎念
真是不搞页面,不知道弄兼容如此的恶心.当然你一板一眼的写,能避免很多的麻烦,可是利用框架,插件来做,这是烦的要死了.

这次利用了 yoesman+gulp+bower 快速搭建了个项目 其中还用到swiper插件 这个插件 3代主要面对手机 可是有些新特性 2代还没有 真是麻烦 用了3代 兼容性就有麻烦了.

# 基本解决问题
bower里有个很好的插件 bootstrap-ie8  `bower install --save bootstrap-ie8`  安装好后 他能帮你解决大多数的兼容问题

# 细微修改

IE 9  不兼容 classList 这个属性 所以需要如下代码
```js
if (!("classList" in document.documentElement)) {
    //兼容ie8  HTMLElement
    window.HTMLElement = window.HTMLElement || Element;
    //兼容ie9 classlist
    Object.defineProperty(HTMLElement.prototype, 'classList', {
      get: function() {
        var self = this;
        function update(fn) {
          return function(value) {
            var classes = self.className.split(/\s+/g),
              index = classes.indexOf(value);

            fn(classes, index, value);
            self.className = classes.join(" ");
          }
        }

        return {
          add: update(function(classes, index, value) {
            if (!~index) classes.push(value);
          }),

          remove: update(function(classes, index) {
            if (~index) classes.splice(index, 1);
          }),

          toggle: update(function(classes, index, value) {
            if (~index)
              classes.splice(index, 1);
            else
              classes.push(value);
          }),

          contains: function(value) {
            return !!~self.className.split(/\s+/g).indexOf(value);
          },

          item: function(i) {
            return self.className.split(/\s+/g)[i] || null;
          }
        };
      }
    });
  }
</script>
```

IE 8 不兼容 window.HTMLElement 所以第一行还得加上这一句 IE浏览器也真是够够的了
```js
    //兼容ie8  HTMLElement
    window.HTMLElement = window.HTMLElement || Element;
```