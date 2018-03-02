---
title: 小程序地图API使用
date: 2018-01-17 22:47:46
tags: 小程序
categories:
---

这次用的比较简单,因为企业名片的项目.需要地图显示标点.
这里只用到了 `wx.openLocation` 其他几个API也比较简单
这里这个会直接打开坐标点,并会标注名车和地址 还会有导航去这里.
```js
  getlocation: function () {
    var that = this;
    wx.openLocation({
      latitude: that.data.latitude,     //经度
      longitude: that.data.longitude,   //纬度
      name: that.data.name,
      address: that.data.address,
      scale: 28
    })
  }
```


## 参考文档
[微信小程序开发之真机测试 地图定位 map API 无法获取当前位置的问题](http://blog.csdn.net/qq_31383345/article/details/53080503)
[微信小程序之地图功能](http://blog.csdn.net/crazy1235/article/details/55004841)
[百度地图微信小程序JavaScript API v1.0](https://github.com/baidumapapi/wxapp-jsapi)
