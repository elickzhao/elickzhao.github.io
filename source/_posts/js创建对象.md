---
title: js创建对象
tags: js
categories: 前端技术
abbrlink: 25823
date: 2016-10-15 23:31:45
---

```js
function bb(){
    alert('bb');
}
var b = new bb();

function cc(){
    return 1;
}

var dd=function(){  
   return "啊打";  
};


function ee(){
    this.name="李小龙";  
    this.age="30";  
}

var e = new ee();

console.log(typeof(aa));
console.log(typeof(bb));
console.log(typeof(b));
console.log(typeof(cc));
console.log(cc());
console.log(typeof(dd));
console.log(typeof(ee));
console.log(typeof(e));

//结果
object      
function
object
function
1
function
function
object

```
[Javascript进阶之路-论对象的重要性 ****](http://www.cnblogs.com/w-wanglei/p/5672167.html)

[js中创建对象的几种方式示例介绍](http://www.jb51.net/article/46249.htm)
[JS函数与面向对象](http://www.cnblogs.com/koeltp/archive/2012/09/08/2676028.html)
