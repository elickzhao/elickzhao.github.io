---
title:  php 类(class)的趣味理解
date: 2016-12-03 22:43:22
tags: php
categories:
---

## 类是什么
类是抽象的一个定义,只是定义不能用.
**class.php**
```php
<?php    

//枪的类
class gun{
    // 弹匣不同的枪有不同弹匣和装弹数量
    protected $magazine ='';

    //初始化弹夹,也就是给抢上弹夹
    public function  __construct(int $magazine=0)
    {
        $this->magazine = $magazine;
    }

    //开火,单发点射.消耗弹夹子弹量.
    public function fire(){
        $this->magazine--;
    }

    public function getMagazine(){
        return $this->magazine;
    }
}

<!--more-->

//游戏开始 先买一把AWP
//echo 'I am AWP';  //有趣带 , 的 echo 前面不能有 echo 否则不显示
$AWP = new gun(10);
echo "Use AWP\n",$AWP->getMagazine(),"\n";
$AWP->fire();
echo $AWP->getMagazine(),"\n";


//再买一把AK
$AK47 = new gun(30);
echo "Use AK\n",$AK47->getMagazine(),"\n";
$AK47->fire();
$AK47->fire();
$AK47->fire();
echo $AK47->getMagazine(),"\n";
```
运行命令 `php class.php` 得输出结果
```
Use AWP
10
9
Use AK
30
27

```
>从上面的例子可以看出,类和类的实例是一个什么样的关系.
类就是一项事物的抽象总结.他是个概念,不能用于实际操作,只是定义了一类物品统一的属性与功能.
上面`枪`类的属性就是`弹夹`,功能就是`开火`.
而实例呢就是真家伙了.利用类`枪`类这个图纸,做出了`AWP`和`AK`.这两个是两个家伙,虽然有共同的属性和方法,但是他们的确是两个家伙,所以开火以后剩余的子弹数是不一样的.所以说为什么`类`是个图纸,如果两个地方用到类,属性是同一个那岂不完蛋.
**所以要记住,类是图纸,实例才是工具**
