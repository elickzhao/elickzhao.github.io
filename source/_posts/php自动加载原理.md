---
title: php自动加载原理
date: 2017-11-27 21:44:41
tags: php
categories:
---

**目录结构**
>F:.
│  Power.php
│  Superman.php
│
└─IoC
        Power.php


**Superman.php**
```php
<?php
namespace IoC;
//use Power\Power; //定义了命名空间也需要引入???? 不能用use自动加载?
//已经确定了 必须的引入 框架里都是使用了自动加载程序 所以无需引用
//比如composer
//include_once 'Power.php';

//两点需要注意的
//一 autoload不能在有命名空间下使用
//二 函数是不能再类里面写的 要么写在之前 要么写在方法里
//spl_autoload_register现在建议用这个函数 不建议使用__autoload了 好像7.2就要把他删掉了
//这两个存在哪一个都可以
//不过这是加载相同命名空间下的文件
spl_autoload_register(function($class){
    if($class){
        $file = $class.'.php';
        if(file_exists($file)){
            include_once $file;
        }
    }
});

class Superman{
    protected $power;

    public function __construct()
    {
        // spl_autoload_register(function($class){
        //     if($class){
        //         $file = $class.'.php';
        //         if(file_exists($file)){
        //             include_once $file;
        //         }
        //     }
        // });
        //所以这个是IoC/下的Power 而不是和Superman同目录下的Power  因注册的是相同命名空间IoC下的文件    
        $this->power = new Power(999,100);
    }


//    public function makeNew(){
//        $this->power = new Power(999,100);
//    }

    public function getPower(){
        //var_dump($this->power);
        return $this->power;
    }


}


//明白了这个函数链是怎么回事了
$man = new Superman;
echo $man->getPower()->getAbility();
echo "<br>";
//这个class的用法 必须没经过实例化的类 经过实例化的类是动态的会报错
echo Power::class;
```

<!--more-->

**Power.php**

```php
<?php
namespace IoC;
//namespace Power;

class Power{
    protected $ability;

    protected $range;

    public function __construct($ability,$range)
    {
        $this->ability = $ability;
        $this->range = $range;
    }

    public function getAbility(){
        return $this->ability;
    }
}
```

自动加载使用的是`spl_autoload_register()`现在不建议使用`__autoload()`了
`spl_autoload_register('类名')`这个函数可以多次加载这比`__autoload()`只能用一次方便很多利于扩展
而且可以在类中使用就想上面那个例子,而'__autoload()'就不可以了
下面这个例子更好的说明了使用方式
```php
<?php
/**
 * Created by PhpStorm.
 * User: chengtao
 * Date: 14-7-3
 * Time: 上午11:27
 */
 
 
define('BASE_PATH',dirname(__FILE__).'/') ;
 
function cron_autoload1 ($name) {
    $file = BASE_PATH.'lib1/'.$name.'.class.php';
    if(file_exists($file)){
        include_once($file);
        return true;
    }
 
}
function cron_autoload2 ($name) {
    $file = BASE_PATH.'lib2/'.$name.'.class.php';
    if(file_exists($file)){
        include_once($file);
        return true;
    }
}
spl_autoload_register('cron_autoload1');
spl_autoload_register('cron_autoload2');
 
new Class1();
new Class2();
```

所以只要有个单独的文件写这些注册的类的地址,然后只要引入这一个文件就好了
以后可以扩展那个文件了
这样就可以实现所有的自动加载.
也就是我最初的疑问,还以为只要use 类 就能自己加载呢 其实是两码事 没那么智能 呵呵
而且命名空间和存放文件地址也未必是一定相对应的,比如最上面第一个例子 Superman的命名空间和地址就不是对应的