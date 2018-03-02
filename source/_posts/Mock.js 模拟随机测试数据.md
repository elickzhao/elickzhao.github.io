---
title: Mock.js 模拟随机测试数据
date: 2017-07-01 22:22:56
tags: [js,前端,mock]
categories:
---

这个东西用起来简单并且强大,不过现在可能不太流行了.因为这个配合jquery比较多.

直接随机产生数据
```js
<script src="http://mockjs.com/dist/mock.js"></script>
<script>
// 使用 Mock
var data = Mock.mock({
    'list|1-10': [{
        'id|+1': 1
    }]
});
$('<pre>').text(JSON.stringify(data, null, 4))
.appendTo('body')
</script>
```

```
{
"list": [
    {
        "id": 1
    },
    {
        "id": 2
    },
    {
        "id": 3
    }
    ]
}
```

<!--more-->

这个是拦截Ajax,返回模拟数据.
```js
Mock.mock('http://g.cn', {
    'name'     : '@name',
    'age|1-100': 100,
    'color'    : '@color'
});

$.ajax({
    url: 'http://g.cn',
    dataType:'json'
    }).done(function(data, status, xhr){
    console.log(
    JSON.stringify(data, null, 4)
    )    
})；


//输出结果
----------------------------
// 结果1
{
"name": "Elizabeth Hall",
"age": 91,
"color": "#0e64ea"
}

// 结果2
{
"name": "Michael Taylor",
"age": 61,
"color": "#081086"
}

```


[官方的 JSFiddle 例子 不明白可以看看这个](http://jsfiddle.net/nzcsxd76/)

## 参考文档
[使用Mock.js进行独立于后端的前端开发](https://segmentfault.com/a/1190000003087224#articleHeader11)
[官方文档](http://mockjs.com/examples.html)
