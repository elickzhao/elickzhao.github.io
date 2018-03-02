---
title: 微信小程序使用自定义icon
date: 2018-01-12 23:53:39
tags: 小程序
categories:
---


使用阿里巴巴的iconfont最简单.
直接下载下来,把压缩包里的css改成wxss 引用就能用了

**wxss**
```css
@import "/dist/weui-grid.wxss";

/* 字体加粗  */

.font-bold {
  font-weight: 700;
}
....
```

**wxml**
```html
<view class="iconfont icon-map zan-c-gray"></view> 
```


## 参考文档
[微信小程序如何引用iconfont图标](https://jingyan.baidu.com/article/14bd256e4bd36bbb6c26126e.html)
[Ionic使用Iconfont-阿里巴巴矢量图标库](https://jingyan.baidu.com/article/14bd256e4bd36bbb6c26126e.html)



