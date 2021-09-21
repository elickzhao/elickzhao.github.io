---
title: js截取字符串到最后几位
tags: js
categories: 前端技术
abbrlink: 16547
date: 2016-06-26 21:29:41
---
这里唯一要点是 substr() 是截取字符串,不过和php不同的是 第二个参数不能为负数,也就是不能倒着数.
所以只能用字符串长度减去要减掉的长度,得到截取的位置

```js
var uri = 'F:\phpStudy\WWW\kod\static\js\lib\editormd\css';
uri.substr(0,(uri.length-11))
```