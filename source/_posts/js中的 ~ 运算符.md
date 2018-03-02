---
title: js中的 ~ 运算符
date: 2016-12-11 22:31:54
tags: js
categories:
---

今天遇到这个 `!~id.indexOf('category-detail')` 以前没碰到过,不是很理解.后来一查明白了,这是位移运算符.

**运算符就是完成操作的一系列符号，它有七类**：
赋值运算符 `=,+=,-=,*=,/=,%=,<<=,>>=,|=,&=`、
算术运算符 `+,-,*,/,++,--,%`、
比较运算符 `>,<,<=,>=,==,===,!=,!==`、
逻辑运算符 `||,&&,!`、
条件运算   `?:`、
位移运算符 `|,&,<<,>>,~,^`
字符串运算符 `+`。

看下面例子更容易理解些,就不用多解释了

<iframe width="100%" height="380" src="http://code.hcharts.cn/test123/dMe0in/share/result,js" allowfullscreen="allowfullscreen" frameborder="0"></iframe>
