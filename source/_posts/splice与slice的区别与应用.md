---
title: splice与slice的区别与应用
date: 2017-04-01 22:40:11
tags: [js,小百科]
categories:
---

slice(开始位置,结束位置) 	//选取元素  //返回选取元素
splice(开始位置,要删除个数,替换元素[可选]) //删除一些元素   //不返回,会把原数组改变
`splice(开始位置,要删除个数,替换元素[可选]) 这个是删除后返回原数组 而slicen()根本不改变原数组 而是将选取赋予一个变量才行 var arr1 = arr.slicen(1,2);`

**运行结果**
<iframe width="100%" height="450" src="http://code.hcharts.cn/blog-demo/FVPmGQ/share/result,js" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

参考文档
[http://www.w3school.com.cn/tiy/t.asp?f=jseg_splice](http://www.w3school.com.cn/tiy/t.asp?f=jseg_splice)
[http://www.w3school.com.cn/tiy/t.asp?f=jseg_slice_array](http://www.w3school.com.cn/tiy/t.asp?f=jseg_slice_array)

[JavaScript splice() 方法](http://www.w3school.com.cn/jsref/jsref_splice.asp)
[JavaScript slice() 方法](http://www.w3school.com.cn/jsref/jsref_slice_array.asp)
