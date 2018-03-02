---
title: CSS3之Transition（变换过渡）使用
date: 2017-08-05 22:17:21
tags: [css,小百科]
categories:
---

```css
transition: property duration timing-function delay;
/*
property：执行过渡的属性        all 或者 width 这些css属性
duration：执行过渡的持续时间    s 或者 ms
timing-function：执行过渡的速率模式   linear：（匀速） ease-in-out （加速然后减速）
delay：延时多久执行 单位为s（秒）或者ms（毫秒），默认就是0，也就是立即执行， 多个动画执行 一个和另一个之间的时间间隔
*/


例子
.ivu-col {
    /* 修改了速度 和 模式 现在看来没有以前的效果, 不过观感好多  */
    transition: width .08s ease-out;   
    /*
    property：执行过渡的属性  all 或者  width
    duration：执行过渡的持续时间  可以是 s 或是 ms
    timing-function：执行过渡的速率模式  ease-in-out
    delay：延时多久执行
      */
}


```


<iframe width="100%" height="450" src="http://code.hcharts.cn/blog-demo/9KFlli/share/result,html,css" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

## 参考文档
[CSS3之Transition（变换过渡）](http://www.jianshu.com/p/5354a9042a2a)
