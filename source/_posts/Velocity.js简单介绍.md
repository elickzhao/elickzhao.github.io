---
title: Velocity.js简单介绍
tags: js
abbrlink: 37714
date: 2017-01-09 22:36:53
categories:
---

## 简介
Velocity 是一个简单易用、高性能、功能丰富的轻量级JS动画库。它能和 jQuery 完美协作，并和$.animate()有相同的 API， 但它不依赖 jQuery，可单独使用。 Velocity 不仅包含了 $.animate() 的全部功能， 还拥有：颜色动画、转换动画(transforms)、循环、 缓动、SVG 动画、和 滚动动画 等特色功能。

它比 $.animate() 更快更流畅，性能甚至高于 CSS3 animation， 是 jQuery 和 CSS3 transition 的最佳组合，它支持所有现代浏览器，最低可兼容到 IE8 和 Android 2.3。

Velocity 目前已被数以千计的公司使用在自己的项目中，包括 WhatsApp, Tumblr, Windows, Samsung, Uber 等，这里 Libscore.com 统计了哪些站点正使用 velocity.js。


**参数概述和基础写法**
Velocity 接收一组 css 属性键值对 (css map) 作为它的第一个参数，该参数作为动画效果的最终属性。第二个参数是可选参数 为动画的额外配置项。下面为参数的写法：

```js
$element.velocity({
    width: "500px",        // 动画属性 宽度到 "500px" 的动画
    property2: value2      // 属性示例
}, {
    /* Velocity 动画配置项的默认值 */
    duration: 400,         // 动画执行时间
    easing: "swing",       // 缓动效果
    queue: "",             // 队列
    begin: undefined,      // 动画开始时的回调函数
    progress: undefined,   // 动画执行中的回调函数（该函数会随着动画执行被不断触发）
    complete: undefined,   // 动画结束时的回调函数
    display: undefined,    // 动画结束时设置元素的 css display 属性
    visibility: undefined, // 动画结束时设置元素的 css visibility 属性
    loop: false,           // 循环
    delay: false,          // 延迟
    mobileHA: true         // 移动端硬件加速（默认开启）
});
```
简单就说这些吧,具体看下面例子.更多的看参考文档,因为很难在简单篇幅里简单解释完全,以后遇到什么问题再补充,还有就是这里还需要一点 `easing` 的东西,这可以看[以前的文章](http://elickzhao.github.io/2017/01/jQuery%20Easing%20%E7%AE%80%E5%8D%95%E8%AE%B2%E8%A7%A3/).


<iframe width="100%" height="380" src="https://code.hcharts.cn/test123/I4Vflc/share/result,js,html,css" allowfullscreen="allowfullscreen" frameborder="0"></iframe>


## 参考文档
[Velocity.js介绍](http://www.open-open.com/lib/view/open1435212597935.html)
[Velocity.js中文文档](http://www.mrfront.com/docs/velocity.js/index.html)
[Velocity ui 动作样式](http://codepen.io/collection/tIjGb/)
[极棒的jquery动画切换引擎插件Velocity.js](http://www.jqcool.net/jquery-velocity.html)