---
title: Chrome 控制台console的用法
tags:
  - js
  - 前端
categories: 前端技术
abbrlink: 15003
date: 2016-11-09 22:05:29
---

大家都有用过各种类型的浏览器，每种浏览器都有自己的特色，本人拙见，在我用过的浏览器当中，我是最喜欢Chrome的，因为它对于调试脚本及前端设计调试都有它比其它浏览器有过之而无不及的地方。可能大家对console.log会有一定的了解，心里难免会想调试的时候用alert不就行了，干嘛还要用console.log这么一长串的字符串来替代alert输出信息呢，下面我就介绍一些调试的入门技巧，让你爱上console.log

先的简单介绍一下chrome的控制台，打开chrome浏览器，按f12就可以轻松的打开控制台
![](http://ww1.sinaimg.cn/mw690/6941baebjw1eo6t5dsyh4j211y0k6q7k.jpg)

<!--more-->

大家可以看到控制台里面有一首诗还有其它信息，如果想清空控制台，可以点击左上角那个![](http://images.cnitblog.com/blog/457824/201411/230851146562060.png)来清空，当然也可以通过在控制台输入console.clear()来实现清空控制台信息。如下图所示
![](http://ww4.sinaimg.cn/mw690/6941baebjw1eo6t5c40imj20il04jgm4.jpg)


现在假设一个场景，如果一个数组里面有成百上千的元素，但是你想知道每个元素具体的值，这时候想想如果你用alert那将是多惨的一件事情，因为alert阻断线程运行，你不点击alert框的确定按钮下一个alert就不会出现。

下面我们用console.log来替换，感受一下它的魅力。
![](http://ww3.sinaimg.cn/mw690/6941baebjw1eo6t5bd0ymj20o905jabn.jpg)


看了上面这张图，是不是认识到log的强大之处了，下面我们来看看console里面具体提供了哪些方法可以供我们平时调试时使用。
![](http://ww1.sinaimg.cn/mw690/6941baebjw1eo6t5ae765j20ea0cimz7.jpg)


目前控制台方法和属性有：
```js
 ["$$", "$x", "dir", "dirxml", "keys", "values", "profile", "profileEnd", "monitorEvents", "unmonitorEvents", "inspect", "copy", "clear", "getEventListeners", "undebug", "monitor", "unmonitor", "table", "$0", "$1", "$2", "$3", "$4", "$_"]
```

下面我们来一一介绍一下各个方法主要的用途。

一般情况下我们用来输入信息的方法主要是用到如下四个

1. **console.log** 用于输出普通信息

2. **console.info** 用于输出提示性信息

3. **console.error**用于输出错误信息

4. **console.warn**用于输出警示信息

5. **console.debug**用于输出调试信息

用图来说话
![](http://ww1.sinaimg.cn/mw690/6941baebjw1eo6t59n8uej209w03mdfx.jpg)


console对象的上面5种方法，都可以使用printf风格的占位符。不过，占位符的种类比较少，**只支持字符（%s）、整数（%d或%i）、浮点数（%f）和对象（%o）**四种。

```js
console.log("%d年%d月%d日",2011,3,26);
console.log("圆周率是%f",3.1415926);
```
![](http://ww1.sinaimg.cn/mw690/6941baebjw1eo6t58xe0jj20f9037aao.jpg)

**%o**占位符，可以用来查看一个对象内部情况
```js
var dog = {};
dog.name = "大毛";
dog.color = "黄色";
console.log("%o", dog);
```
![](http://ww4.sinaimg.cn/mw690/6941baebjw1eo6tbbgxqfj2095043jrr.jpg)


6. **console.dirxml**用来显示网页的某个节点（node）所包含的html/xml代码
```js
<body>
    <table id="mytable">
        <tr>
            <td>A</td>
            <td>A</td>
            <td>A</td>
        </tr>
        <tr>
            <td>bbb</td>
            <td>aaa</td>
            <td>ccc</td>
        </tr>
        <tr>
            <td>111</td>
            <td>333</td>
            <td>222</td>
        </tr>
    </table>
</body>
<script type="text/javascript">
    window.onload = function () {
        var mytable = document.getElementById('mytable');
        console.dirxml(mytable);
    }
</script>
```
![](http://ww2.sinaimg.cn/mw690/6941baebjw1eo6tc9egt1j209807kq3e.jpg)


7. **console.group** 输出一组信息的开头

8. **console.groupEnd** 结束一组输出信息

看你需求选择不同的输出方法来使用，如果上述四个方法再配合group和groupEnd方法来一起使用就可以输入各种各样的不同形式的输出信息。

![](http://ww2.sinaimg.cn/mw690/6941baebjw1eo6tdedmvnj20ec04iq38.jpg)

哈哈，是不是觉得很神奇呀！

9. **console.assert** 对输入的表达式进行断言，只有表达式为false时，才输出相应的信息到控制台
![](http://ww1.sinaimg.cn/mw690/6941baebjw1eo6t587ydgj20gx03ddgg.jpg)


10. **console.count**（这个方法非常实用哦）当你想统计代码被执行的次数
![](http://ww1.sinaimg.cn/mw690/6941baebjw1eo6t57cdldj20e306c75a.jpg)


11. **console.dir** (这个方法是我经常使用的 可不知道比for in方便了多少) 直接将该DOM结点以DOM树的结构进行输出，可以详细查对象的方法发展等等
![](http://ww4.sinaimg.cn/mw690/6941baebjw1eo6t56i2p8j20ew0avab2.jpg)


12. **console.time** 计时开始

13. **console.timeEnd**  计时结束（看了下面的图你瞬间就感受到它的厉害了）
![](http://ww4.sinaimg.cn/mw690/6941baebjw1eo6t55rxtej20e808ndhq.jpg)

14. **console.profile**和**console.profileEnd**配合一起使用来查看CPU使用相关信息
![](http://ww3.sinaimg.cn/mw690/6941baebjw1eo6t54ndn6j20e906b0tr.jpg)

在Profiles面板里面查看就可以看到cpu相关使用信息
![](http://ww4.sinaimg.cn/mw690/6941baebjw1eo6t53grshj20iw05vab0.jpg)

15. **console.timeLine**和**console.timeLineEnd**配合一起记录一段时间轴

16. **console.trace**  堆栈跟踪相关的调试

上述方法只是我个人理解罢了。如果想查看具体API，可以上官方看看，具体地址为：https://developer.chrome.com/devtools/docs/console-api



下面介绍一下控制台的一些快捷键

1. **方向键盘的上下键**，大家一用就知晓。比如用上键就相当于使用上次在控制台的输入符号

2. **$_**命令返回最近一次表达式执行的结果，功能跟按向上的方向键再回车是一样的
![](http://ww1.sinaimg.cn/mw690/6941baebjw1eo6t52eylvj20f604sdg3.jpg)

上面的$_需要领悟其奥义才能使用得当，而$0~$4则代表了最近5个你选择过的DOM节点。

什么意思？在页面右击选择审查元素，然后在弹出来的DOM结点树上面随便点选，这些被点过的节点会被记录下来，而$0会返回最近一次点选的DOM结点，以此类推，$1返回的是上上次点选的DOM节点，最多保存了5个，如果不够5个，则返回undefined。
![](http://ww1.sinaimg.cn/large/6941baebjw1eo6t51jeohg20n80cntdf.gif)

3. **Chrome 控制台中原生支持类jQuery的选择器**也就是说你可以用$加上熟悉的css选择器来选择DOM节点
![](http://ww2.sinaimg.cn/mw690/6941baebjw1eo6t4zmpnyj20zo0bojvq.jpg)


4. **copy**通过此命令可以将在控制台获取到的内容复制到剪贴板
![](http://ww3.sinaimg.cn/mw690/6941baebjw1eo6t4y2o5uj20fi03f3ys.jpg)

（哈哈 刚刚从控制台复制的body里面的html可以任意粘贴到哪 比如记事本  是不是觉得功能很强大）

5. **keys和values** 前者返回传入对象所有属性名组成的数据，后者返回所有属性值组成的数组
![](http://ww2.sinaimg.cn/mw690/6941baebjw1eo6t4wuki6j20du05umxv.jpg)

说到这，不免想起console.table方法了
![](http://ww2.sinaimg.cn/mw690/6941baebjw1eo6t4vy093j210y0gzgnj.jpg)


6. **monitor & unmonitor**

monitor(function)，它接收一个函数名作为参数，比如function a,每次a被执行了，都会在控制台输出一条信息，里面包含了函数的名称a及执行时所传入的参数。
而unmonitor(function)便是用来停止这一监听。
![](http://ww4.sinaimg.cn/mw690/6941baebjw1eo6t4uttxoj20f405c74y.jpg)


看了这张图，应该明白了，也就是说在monitor和unmonitor中间的代码，执行的时候会在控制台输出一条信息，里面包含了函数的名称a及执行时所传入的参数。当解除监视（也就是执行unmonitor时）就不再在控制台输出信息了。
```js
$ // 简单理解就是 document.querySelector 而已。
$$ // 简单理解就是 document.querySelectorAll 而已。
$_ // 是上一个表达式的值
$0-$4 // 是最近5个Elements面板选中的DOM元素，待会会讲。
dir // 其实就是 console.dir
keys // 取对象的键名, 返回键名组成的数组
values // 去对象的值, 返回值组成的数组
```


下面看一下console.log的一些技巧

**1、重写console.log 改变输出文字的样式**
![](http://ww1.sinaimg.cn/mw690/6941baebjw1eo6t4txwawj20nt06xjtd.jpg)

**2、利用控制台输出图片**
![](http://ww4.sinaimg.cn/large/6941baebjw1eo6t4tc4szg20mc080758.gif)

**3、指定输出文字的样式**
![](http://ww3.sinaimg.cn/mw690/6941baebjw1eo6t4s44kvj210b04zq56.jpg)

最后说一下chrome控制台一个简单的操作，如何查看页面元素，看下图就知道了
![](http://ww1.sinaimg.cn/mw690/6941baebjw1eo6t4r4nc2j20os0amwh5.jpg)

你在控制台简单操作一遍就知道了，是不是觉得很简单！
