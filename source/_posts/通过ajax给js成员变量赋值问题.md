---
title: 通过ajax给js成员变量赋值问题
tags:
  - js
  - 小百科
abbrlink: 6570
date: 2018-01-28 22:57:37
categories:
---


```js
function getDate(url) {
    var result;
    $.ajax({
        type: 'GET',
        async :false,//取消异步 否则result赋值失败 
        url: '/index.php/Admin/Page/orderChartsData.html',
        success: function (res) {
            //console.log(res);
            result = res;
        },
        error: function (e) {
            console.log(e);
        }
    });
    return result;
}
```

## 参考文档
[通过ajax给js成员变量赋值问题](http://blog.csdn.net/u014225431/article/details/51451498)