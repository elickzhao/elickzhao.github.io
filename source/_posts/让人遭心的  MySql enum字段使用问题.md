---
title: 让人遭心的  MySql enum字段使用问题
tags:
  - mysql
  - 小百科
abbrlink: 3378
date: 2017-12-14 16:44:08
categories:
---
今天在改程序的时候遇到了 enum字段 这个东西果真奇葩.怎么搜索都搜索不到.
后来查了资料才搞懂.
这个字段是 mysql独有 其他数据库都没有的 所以用了他就别想换了
他的唯一优点就是控制大小,因为他是类似数组形式保存的 所以数量是可控的
但也造成了搜索时候的问题. 直接搜索的时候有可能搜索的是按key来搜不是保存在字段里value 唉...


据下面文档说 他的key是从1开始 所以搜不到0 
~~但是我遇到更奇葩的事 就算我按key=1 来搜索也搜不到 0 这个值. 而其他两个 1 和 2 全能搜到~~
妈蛋 原来人家说的是对的 只不过数据库形式我没仔细看错了 搜索3就出来了
数据库形式 是这样的  ['1'=>1,'2'=>'2','3'=>0]
```
SELECT * FROM lr_order WHERE back = 3;  //这是按key搜索 匹配出来的 全是 back=0
```

如果想按值搜索怎么做? 加上单引号就行了
```
SELECT * FROM lr_order WHERE back = '0';  //这是按value搜索 匹配出来的 全是 back=0
```

这个破东西的确烦,可能框架都没法用.所以建立数据库的时候,还是用tinyint算了

##参考文档
[MySql enum字段使用问题](http://blog.csdn.net/kkikiako/article/details/49746791)
[要慎用mysql的enum字段的原因](http://www.jb51.net/article/53426.htm)
