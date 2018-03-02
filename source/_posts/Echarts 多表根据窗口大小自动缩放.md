---
title: Echarts 多表根据窗口大小自动缩放
date: 2018-01-30 22:54:50
tags: [js,小百科,Echarts]
categories:
---


当只有一个图表的时候很简单只要这么写就搞定了

```js
window.onresize = orderCharts.myChart.resize;
```

当多个图表的时候,就会出现只有一个图表能自动缩放,这时就要这么写了.

```js

window.onresize = function () {
    orderCharts.myChart.resize(); //图表自适应窗口大小
    userCharts.myChart.resize(); //图表自适应窗口大小
}

```

这样写就ok了 下面这个写法也是等效的.

```js
window.addEventListener("resize", function () {
    console.log(orderCharts);
    orderCharts.myChart.resize(); //图表自适应窗口大小
    userCharts.myChart.resize(); //图表自适应窗口大小
});
```


**注意**
当一个图标的时候 `resize`是没有括号的, 而多个图标的时候是有括号的. 这个要小心,他们不能互换着用.用时一定要仔细.
