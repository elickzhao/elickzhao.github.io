---
title: Emmet：HTMLCSS 简明介绍
date: 2016-05-13 01:03:28
tags: [Emmet,小百科]
categories: 前端技术
---


这里就写几个主要的,详细请看这篇文章:
[http://www.iteye.com/news/27580](http://www.iteye.com/news/27580)
[使用Emmet加速Web前端开发](http://www.w3cplus.com/tools/using-emmet-speed-front-end-web-development.html)
- 初始化 

HTML文档需要包含一些固定的标签，比如<html>、<head>、<body>等，现在你只需要1秒钟就可以输入这些标签。比如输入“!”或“html:5”，然后按Tab键：

-  嵌套 

现在你只需要1行代码就可以实现标签的嵌套。 
```
>: 子元素符号，表示嵌套的元素
+：同级标签符号
^：可以使该符号前的标签提升一行
```

- 分组 

你可以通过嵌套和括号来快速生成一些代码块，比如输入(.foo>h1)+(.bar>h2)，会自动生成如下代码
```html
<div class="foo">
  <h1></h1>
</div>
<div class="bar">
  <h2></h2>
</div>
```

- 快速添加类名、ID、文本和属性

 1. 使用E#ID添加ID名
 2. 使用E.class添加类名
 3. 使用E[attr]添加属性
 4. 使用E{text}添加文本

<!--more-->

- 隐式标签 

声明一个带类的标签，只需输入div.item，就会生成<div class="item"></div>。 

在过去版本中，可以省略掉div，即输入.item即可生成`<div class="item"></div>`。现在如果只输入.item，则Emmet会根据父标签进行判定。比如在`<ul>`中输入.item，就会生成`<li class="item"></li>`。

- 定义多个元素 

要定义多个元素，可以使用*符号。比如，ul>li*3可以生成如下代码： 
```html
<ul>
  <li></li>
  <li></li>
  <li></li>
</ul>
```

- 定义多个带属性的元素 

如果输入 ul>li.item$*3，将会生成如下代码：

```
<ul>
  <li class="item1"></li>
  <li class="item2"></li>
  <li class="item3"></li>
</ul>
```

# emmet 和 css

- 简写
width: 100px : w100
border-bottom : bdb
border-top: bdt
font-size : fz

- 单位:
     px→ 默认
     p→ %
     e→ em
     r→ rem
     x→ ex

- 多个单位

CSS中的某些属笥，比如margin，允许多个值。在Emmet中要做到这一点，只需要每个值之间使用破折号(-)。来看看下面的例子，给body定义margin的四个值：

颜色

- 在Emmet中使用#前缀，后面紧跟颜色值，但不同的字符数将会输出不同的十六进制代码。来看一些例子：

 ＃1→ ＃111111
 ＃E0→ ＃e0e0e0
 ＃FC0→ ＃FFCC00
 
- !important

尽管在CSS中!important并不经常使用，但在Emmet也带有一定的缩写。添加!就可以自动生成：


- 多属性

现在我们具Emmet的CSS特性的一个基本了解，也是将它们放在一起的时候。就类似于Emmet和HTML中的相邻元素的功能。可以使用加号+运算符来创建多个属性。我们来看一个简单的示例：

- 模糊匹配 

如果有些缩写你拿不准，Emmet会根据你的输入内容匹配最接近的语法，比如输入ov:h、ov-h、ovh和oh，生成的代码是相同的

```
overflow: hidden;
```

- 供应商前缀 

如果输入非W3C标准的CSS属性，Emmet会自动加上供应商前缀，比如输入trs，则会生成： 

```
-webkit-transform: ;  
-moz-transform: ;  
-ms-transform: ;  
-o-transform: ;  
transform: ;  
```

你也可以在任意属性前加上“-”符号，也可以为该属性加上前缀。比如输入-super-foo： 
```
-webkit-super-foo: ;
-moz-super-foo: ;
-ms-super-foo: ;
-o-super-foo: ;
super-foo: ;
```
如果不希望加上所有前缀，可以使用缩写来指定，比如-wm-trf表示只加上-webkit和-moz前缀： 
```
-webkit-transform: ;
-moz-transform: ;
transform: ;
```
w 表示 -webkit-
m 表示 -moz-
s 表示 -ms-
o 表示 -o-

-  渐变 

输入lg(left, #fff 50%, #000)，会生成如下代码： 
```
background-image: -webkit-gradient(linear, 0 0, 100% 0, color-stop(0.5, #fff), to(#000));
background-image: -webkit-linear-gradient(left, #fff 50%, #000);
background-image: -moz-linear-gradient(left, #fff 50%, #000);
background-image: -o-linear-gradient(left, #fff 50%, #000);
background-image: linear-gradient(left, #fff 50%, #000);
```