---
title: 小程序中引用import和include区别
tags: 小程序
abbrlink: 54568
date: 2017-12-21 22:11:52
categories:
---


先说这两个的主要用法吧
**import** 引入模版 可以根据固定样式显示数据 所以适合和数据进行组合的
**include** 引入内容 无法和数据进行组合 所以适合引用固定样式的组件 而没有与数据交换操作的页面

---

~~现在很奇怪的事情是 include的不好使. 只有import是可用的~~
已经搞懂include怎么用了,这个文档啊,字句真需要仔细琢磨.要不太有歧义了.
>include可以将目标文件除了<template/>的整个代码引入，相当于是拷贝到include位

应该是 除了`<template/>`代码块内的,其余内容全部引入.

**include**这么用
```
<include src="/pages/template/tabbar.wxml" />

---

  <view class="weui-tabbars">
    <navigator url="/pages/index/index" open-type='switchTab' class="weui-tabbar">
      <image class="weui-tabbar__icon" src="../../images/icons/a.png" />
      <view class="weui-tabbar__label">首页</view>
    </navigator>
    <navigator url="/pages/category/index" open-type='switchTab' class="weui-tabbar">
      <image class="weui-tabbar__icon" src="../../images/icons/b.png" />
      <view class="weui-tabbar__label">分类</view>
    </navigator>
    <navigator url="/pages/cart/cart" open-type='switchTab' class="weui-tabbar">
      <image class="weui-tabbar__icon" src="../../images/icons/c.png" />
      <view class="weui-tabbar__label">购物车</view>
    </navigator>
    <navigator url="/pages/user/user" open-type='switchTab' class="weui-tabbar">
      <image class="weui-tabbar__icon" src="../../images/icons/dd.png" />
      <view class="weui-tabbar__label" style='color: #FF8140'>我的</view>
    </navigator>
  </view>
```



**import**这么用
```
    <import src="../template/tabbar.wxml" />
    <template is="tabbar" />
----

<template name="tabbar">
  <view class="weui-tabbars">
    <navigator url="/pages/index/index" open-type=&#39;switchTab&#39; class="weui-tabbar">
      <image class="weui-tabbar__icon" src="../../images/icons/a.png" />
      <view class="weui-tabbar__label">首页</view>
    </navigator>
    <navigator url="/pages/category/index" open-type=&#39;switchTab&#39; class="weui-tabbar">
      <image class="weui-tabbar__icon" src="../../images/icons/b.png" />
      <view class="weui-tabbar__label">分类</view>
    </navigator>
    <navigator url="/pages/cart/cart" open-type=&#39;switchTab&#39; class="weui-tabbar">
      <image class="weui-tabbar__icon" src="../../images/icons/c.png" />
      <view class="weui-tabbar__label">购物车</view>
    </navigator>
    <navigator url="/pages/user/user" open-type=&#39;switchTab&#39; class="weui-tabbar">
      <image class="weui-tabbar__icon" src="../../images/icons/dd.png" />
      <view class="weui-tabbar__label" style=&#39;color: #FF8140&#39;>我的</view>
    </navigator>
  </view>
</template>
```

## 参考文档
[小程序中引用import和include区别](http://blog.csdn.net/sinat_39343982/article/details/74095205)

