---
title: jquery 获取onclick() 当前点击对象
date: 2018-01-29 14:41:31
tags: [js,小百科]
categories:
---


必须传入this. **而且在方法内使用时 记住不能用this**
这样就可以使用`$`获取当前对象了

```js
<span class="btn btn-primary radius" onclick="t(this)">今日</span> 

function t(obj) {
    changeGroupBtn(obj);
    option.xAxis.data = dayArray;
    option.series[0].data = getDate(getUrl, 'today'); 
    myChart.setOption(option);
}


/*=============================================
=            按钮组切换按钮                    =
=============================================*/
function changeGroupBtn(obj) {
    $(obj).siblings().removeClass("btn-primary").addClass("btn-default");   //同胞元素
    $(obj).removeClass("btn-default").addClass("btn-primary");
}
/*=====  End of 按钮组切换按钮  ======*/

## 参考文档
[onclick中，获取不了$(this)](http://blog.csdn.net/catherian/article/details/53906978)