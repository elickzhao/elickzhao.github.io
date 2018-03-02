---
title: CSS的float正确理解及overflowhidden 属性详细解释
date: 2016-07-03 19:01:18
tags: [css,小百科,前端]
categories: 前端技术
---

## 简单说明
**float**
原来一直以为float是平面的,浮动就是左右偏移在平面上运动,其实是错的,浮动是立体的,
记得用火狐浏览器的debug工具,有一项就是能看到立体的样子.
所以当设置浮动后,这个层的宽高就不在瘦父层级的限制了,因为他是立体的,他实际是飘在父层级上.
所以需要清浮动才可以让他继续接受父层级的控制,这里就需要`overflow:hidden`

**overflow:hidden**
所以说 `overflow:hidden` 并不是我们原以为的简单只是把多出的内容隐藏,他还有个引申的功能就是清理浮动
所以当你使用float的时候一定要加上这个,要不然容易造成margin的偏移,或者使用padding解决这个问题.或者是子层级不受控制超出了父层级.

## 参考文档
[CSS 的overflow:hidden 属性详细解释](http://jingyan.baidu.com/article/d45ad148e2a7f969552b80ae.html)
