---
title: css伪类 first-child  伪元素   before
date: 2016-11-18 10:38:22
tags: css
categories:
---

## 简介
  在使用mui时,遇到 ul li 就会有下边框,可是又跟 border 没有关系, 只要去掉 position: relative 就可以了. 刚开始图省事就这么做了,可后面的问题多多.最后不得已翻看源码才知道,这个东西是拿伪类弄出来的.其实以前也见过,就是没仔细研究,这次遇到了,所以弄明白点了.

## 讲解

先讲这两个,比较常见的.

**":before"** 伪元素可以在元素的内容前面插入新内容。
**":after"** 伪元素可以在元素的内容后面插入新内容。

```css
/*
下面就是困扰我的那行代码,通过变形拉伸,露出页面底色,看上去就是绘制出一个横线.
*/
.mui-table-view
{
    position: relative;

    margin-top: 0;
    margin-bottom: 0;
    padding-left: 0;

    list-style: none;

    background-color: #fff;
}
.mui-table-view:after
{
    position: absolute;
    right: 0;
    bottom: 0;
    left: 0;

    height: 1px;

    content: '';
    -webkit-transform: scaleY(.5);
            transform: scaleY(.5);

    background-color: #c8c7cc;
}
.mui-table-view:before
{
    position: absolute;
    top: 0;
    right: 0;
    left: 0;

    height: 1px;

    content: '';
    -webkit-transform: scaleY(.5);
            transform: scaleY(.5);

    background-color: #c8c7cc;
}
```
<!--more-->

**:first-child** 伪类来选择元素的第一个子元素。
**:last-child** 伪类来选择元素的最后一个子元素。
**:nth-last-child(3)** 伪类来选择元素的倒数第三个子元素。

上面这俩很好理解,不展示例子了.关键的来了,如果又是第一个元素,又是元素之后改变如何写呢,看下面.

```html
<html>
	<head>
		<title>导航条</title>
		<style>
		ul {
                list-style-type: none;
                text-align: center;
            }
            li {
                display: inline;
            }
            li:not(:last-child):after {     /*这里的 not 是除了最后一个以外,所有元素*/
                content:' |';
            }
		</style>
	</head>
	<body>
	<ul>
        <li>One</li>
        <li>Two</li>
        <li>Three</li>
        <li>Four</li>
        <li>Five</li>
    </ul>
	</body>
</html>
```
这里是演示地址
<iframe width="100%" height="380" src="http://code.hcharts.cn/test123/IHE0nk/share/result,html,css" allowfullscreen="allowfullscreen" frameborder="0"></iframe>


更多的选择器,请看手册:
[CSS 选择器参考手册](http://www.w3school.com.cn/cssref/css_selectors.asp)
[CSS 伪类 (Pseudo-classes)](http://www.w3school.com.cn/css/css_pseudo_classes.asp)
[CSS 伪元素 (Pseudo-elements)](http://www.w3school.com.cn/css/css_pseudo_elements.asp)
[算法组](http://suanfazu.com/t/css-not-last-child-after-selector/9330)
