---
title: ECharts x坐标系使用 time 形式
tags:
  - js
  - 小百科
abbrlink: 36230
date: 2018-01-14 22:50:28
categories:
---

xAxis.type string
[ default: 'category' ]
坐标轴类型。
可选：
'value' 数值轴，适用于连续数据。
'category' 类目轴，适用于离散的类目数据，为该类型时必须通过 data 设置类目数据。
'time' 时间轴，适用于连续的时序数据，与数值轴相比时间轴带有时间的格式化，在刻度计算上也有所不同，例如会根据跨度的范围来决定使用月，星期，日还是小时范围的刻度。
'log' 对数轴。适用于对数数据。
```

一般情况都会使用`category` 因为比较简单 直接传入数据就能用
**例如这样**
```
option = {
    title: {
        text: '未来一周气温变化',
        subtext: '纯属虚构'
    },
    tooltip: {
        trigger: 'axis'
    },
    toolbox: {
        show: true,
        feature: {
            dataZoom: {
                yAxisIndex: 'none'
            },
            dataView: {readOnly: false},
            magicType: {type: ['line', 'bar']},
            restore: {},
            saveAsImage: {}
        }
    },
    xAxis:  {
        type: 'category',
        boundaryGap: false,
        data: ['周一','周二','周三','周四','周五','周六','周日']
    },
    yAxis: {
        type: 'value',
        axisLabel: {
            formatter: '{value} °C'
        }
    },
    series: [
        {
            name:'最高气温',
            type:'line',
            data:[11, 11, 15, 13, 12, 13, 10],
            markPoint: {
                data: [
                    {type: 'max', name: '最大值'},
                    {type: 'min', name: '最小值'}
                ]
            },
            markLine: {
                data: [
                    {type: 'average', name: '平均值'}
                ]
            }
        }
    ]
};

```
<!--more-->

但是time呢 他的文档里没有说的特别清楚 这里给出个例子
传入的数据必须带一个时间的item 而且好像光是日期还不行 必须是完整的
还有就是必须是value字段 好像是这个样子
```js
var data = [
    {value:['2016/12/18 6:38:08', 80]},
    {value:['2016/12/18 16:18:18', 60]},
    {value:['2016/12/18 19:18:18', 90]}
    ];


    option = {
    title: {
        text: '未来一周气温变化',
        subtext: '纯属虚构'
    },
    tooltip: {
        trigger: 'axis'
    },
    toolbox: {
        show: true,
        feature: {
            dataZoom: {
                yAxisIndex: 'none'
            },
            dataView: {readOnly: false},
            magicType: {type: ['line', 'bar']},
            restore: {},
            saveAsImage: {}
        }
    },
    xAxis:  {
        type: 'time',
        boundaryGap: false,
    },
    yAxis: {
        type: 'value',
        axisLabel: {
            formatter: '{value} °C'
        }
    },
    series: [
        {
            name:'最高气温',
            type:'line',
            data:data,
            markPoint: {
                data: [
                    {type: 'max', name: '最大值'},
                    {type: 'min', name: '最小值'}
                ]
            },
            markLine: {
                data: [
                    {type: 'average', name: '平均值'}
                ]
            }
        }
    ]
};
```

好了更多具体使用看文档 他的文档还是比较详细的

## 参考文档
[Echarts文档](http://echarts.baidu.com/option.html#xAxis.type)
[ECharts显示24小时时间数据的一种办法](https://www.2cto.com/kf/201612/577871.html)



