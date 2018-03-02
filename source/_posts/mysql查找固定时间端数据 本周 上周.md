---
title: mysql查找固定时间端数据 本周 上周...
date: 2018-01-15 22:29:06
tags: [mysql,php]
categories:
---

mysql有很多关于时间截取的方法,不过呢数据格式必须date形式的,不过我现在用的int保存时间戳的方式.
以前记得好像date形式有什么缺点 现在给忘记了 以后遇到再记录下吧.

函数类似这种 这次是没法测试 以后数据库可以参考下 好像这种更优化一些
```
SELECT OrderId,DATE_SUB(OrderDate,INTERVAL 5 DAY) AS SubtractDate FROM Orders
```

[MySQL DATE_SUB() 函数](https://www.w3cschool.cn/mysql/func-date-sub.html)
[mysql获取昨天的时间](http://blog.csdn.net/wocjj/article/details/7415253)
[MySQL查询本周、上周、本月、上个月份数据的sql代码](http://www.thinkphp.cn/topic/47049.html)