---
title: 'php 重载相关魔法函数 __get()，__set()，__isset() , __unset()  __call() __callStatic'
tags:
  - php
  - 小百科
categories: php开发
abbrlink: 32143
date: 2016-06-20 22:32:59
---

## 前言
  从这里开始
```php
      public function __call($method, $parameters)
    {
        //这里注意 $this->guard() 返回的是个实例,call_user_func_array()第一个参数如果是类,
        //则需要数组第一个是类的实例变量
        return call_user_func_array([$this->guard(), $method], $parameters);
    }
```

<!--more-->

## 简单说明

>PHP所提供的"重载"（overloading）是指动态地"创建"类属性和方法。我们是通过魔术方法（magic methods）来实现的。

属性重载:  `__get() __set() __isset()   __unset()`
类重载: `__call() __callStatic`

**属性重载使用如下**
```php
class PropertyTest {
     /**  被重载的数据保存在此  */
    private $data = array();

 
     /**  重载不能被用在已经定义的属性  */
    public $declared = 1;

     /**  只有从类外部访问这个属性时，重载才会发生 */
    private $hidden = 2;

    public function __set($name, $value) 
    {
        echo "Setting '$name' to '$value'\n";
        $this->data[$name] = $value;
    }

    public function __get($name) 
    {
        echo "Getting '$name'\n";
        if (array_key_exists($name, $this->data)) {
            return $this->data[$name];
        }

        $trace = debug_backtrace();
        trigger_error(
            'Undefined property via __get(): ' . $name .
            ' in ' . $trace[0]['file'] .
            ' on line ' . $trace[0]['line'],
            E_USER_NOTICE);
        return null;
    }

    /**  PHP 5.1.0之后版本 */
    public function __isset($name) 
    {
        echo "Is '$name' set?\n";
        return isset($this->data[$name]);
    }

    /**  PHP 5.1.0之后版本 */
    public function __unset($name) 
    {
        echo "Unsetting '$name'\n";
        unset($this->data[$name]);
    }

    /**  非魔术方法  */
    public function getHidden() 
    {
        return $this->hidden;
    }
}


echo "<pre>\n";

$obj = new PropertyTest;

$obj->a = 1;
echo $obj->a . "\n\n";

var_dump(isset($obj->a));
unset($obj->a);
var_dump(isset($obj->a));
echo "\n";

echo $obj->declared . "\n\n";

echo "Let's experiment with the private property named 'hidden':\n";
echo "Privates are visible inside the class, so __get() not used...\n";
echo $obj->getHidden() . "\n";
echo "Privates not visible outside of class, so __get() is used...\n";
echo $obj->hidden . "\n";

$obj->hidden=333;       //这里需要注意下,设置的并不是属性hidden,所以得到也并不是.
echo $obj->hidden . "\n";
echo $obj->getHidden() . "\n";
```

```
## 输出结果如下

Setting 'a' to '1'
Getting 'a'
1

Is 'a' set?
bool(true)
Unsetting 'a'
Is 'a' set?
bool(false)

1

Let's experiment with the private property named 'hidden':
Privates are visible inside the class, so __get() not used...
2
Privates not visible outside of class, so __get() is used...
Getting 'hidden'


Notice:  Undefined property via __get(): hidden in F:\phpStudy\WWW\kod\data\public\test\1.php on line 134 in F:\phpStudy\WWW\kod\data\public\test\1.php on line 90


Setting 'hidden' to '333'
Getting 'hidden'
333
2
```

**方法重载使用如下**

```php
//小例子
//获得每个城市天气预报
class Action{

public function tj(){
 echo 'tj天气预报<br/>';
 }

/*
$m 方法名
$p 方法参数集合
*/
public function __call($m,$p){

  echo $m,'天气预报<br/>';
  }

}

$c=new Action();
$c->tj();

//获得城市
$city=$_GET['method'];


if(isset($city)){

//获得城市的方法，由魔术方法__call处理
$c->$city();

}
/*
网址:http://localhost/php/60.php?method=beijing
结果：
tj天气预报
beijing天气预报
*/


?>
```


```php
class MethodTest 
{
    public function __call($name, $arguments) 
    {
        // 注意: $name 的值区分大小写
        echo "Calling object method '$name' "
             . implode(', ', $arguments). "\n";
    }

    /**  PHP 5.3.0之后版本  */
    public static function __callStatic($name, $arguments) 
    {
        // 注意: $name 的值区分大小写
        echo "Calling static method '$name' "
             . implode(', ', $arguments). "\n";
    }
}

$obj = new MethodTest;
//runTest方法不存在
$obj->runTest('in object context');

MethodTest::runTest('in static context');  // PHP 5.3.0之后版本
-----
//输出如下
/*
Calling object method 'runTest' in object context
Calling static method 'runTest' in static context
*/

```

**过载实现**
```php
class Magic {
  function __call($name,$arguments) {
    if($name=='foo') {
        if(is_int($arguments[0])) $this->foo_for_int($arguments[0]);
        if(is_string($arguments[0])) $this->foo_for_string($arguments[0]);
    }
  } 
  private function foo_for_int($x) {
        print("oh an int!");
  }  
  private function foo_for_string($x) {
        print("oh a string!");
  }
} 
$x = new Magic();
$x->foo(3);
$x->foo("3");

-----
//输出如下
/*
oh an int!
oh a string!
*/
```

## 参考文档

[官方文档](http://php.net/manual/zh/language.oop5.overloading.php#object.call)
[PHP魔术方法之__call与__callStatic方法](http://blog.csdn.net/binghui1990/article/details/9104685)
[PHP5学习笔记：用__call()实现方法重载](http://www.cnblogs.com/xiaochaohuashengmi/archive/2011/09/22/2185032.html)