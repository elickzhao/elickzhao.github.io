---
title: php 函数学习
tags: php
categories: php开发
abbrlink: 62936
date: 2016-06-01 23:21:40
---

**查看练习地址**:
[https://ide.c9.io/elickzhao/test(https://ide.c9.io/elickzhao/test) 

[TOC]

<!--more-->

## 魔术常量

|  名称 | 说明  |
| ------------ | ------------ |
| `__FILE__ ` |  文件的完整路径和文件名。如果用在被包含文件中，则返回被包含的文件名。自 PHP 4.0.2 起，`__FILE__` 总是包含一个绝对路径（如果是符号连接，则是解析后的绝对路径），而在此之前的版本有时会包含一个相对路径。 |
|  `__DIR__` | 文件所在的目录。如果用在被包括文件中，则返回被包括的文件所在的目录。它等价于 `dirname(__FILE__)`。除非是根目录，否则目录中名不包括末尾的斜杠。（PHP 5.3.0中新增） 。  |
|  `__FUNCTION__` | 函数名称（PHP 4.3.0 新加）。自 PHP 5 起本常量返回该函数被定义时的名字（区分大小写）。在 PHP 4 中该值总是小写字母的。  |
|  `__CLASS__` | 类的名称（PHP 4.3.0 新加）。自 PHP 5 起本常量返回该类被定义时的名字（区分大小写）。在 PHP 4 中该值总是小写字母的。类名包括其被声明的作用区域（例如 `Foo\Bar`）。注意自 PHP 5.4 起 `__CLASS__` 对 `trait` 也起作用。当用在 `trait` 方法中时，`__CLASS__` 是调用 `trait` 方法的类的名字。。  |
|  `__TRAIT__` | `Trait` 的名字（PHP 5.4.0 新加）。自 PHP 5.4 起此常量返回 `trait` 被定义时的名字（区分大小写）。`Trait` 名包括其被声明的作用区域（例如 Foo\Bar）。  |
|  `__METHOD__` | 类的方法名（PHP 5.0.0 新加）。返回该方法被定义时的名字（区分大小写）。。  |
|  `__NAMESPACE__` | 当前命名空间的名称（区分大小写）。此常量是在编译时定义的（PHP 5.3.0 新增）。  |


## A
### array_slice()
```
//从数组中取出一段
//参数 取出开始位置 取出个数
//返回是个数组
array_slice()
```
## C
### class_uses()
```
//返回类所引用的trait  
//参数可以是个 实例 new bar  也可以是个字符串类名 'bar'
//返回的是一个数组结果 
class_uses()
```
### checkdnsrr()
```
//主要用于检查域名和ip是否正确
//给指定的主机（域名）或者IP地址做DNS通信检查
//参数 ip或者域名 备选参数主机类型 
//返回 一个布尔值
//bool checkdnsrr ( string $host [, string $type = "MX" ] )
checkdnsrr()
```

### call_user_func_array()
```
mixed call_user_func_array ( callable $callback , array $param_arr )
```

### compact()
创建一个包含变量名和它们的值的数组：
```php
<?php
$firstname = "Bill";
$lastname = "Gates";
$age = "60";

$result = compact("firstname", "lastname", "age");

print_r($result);
?>

-----
out:
Array ( [firstname] => Bill [lastname] => Gates [age] => 60 )
```

## D
### dirname()
```php
//dirname() 函数返回路径中的目录部分。
dirname(path);

echo dirname("c:/testweb/home.php");
echo dirname("/testweb/home.php");

------
#output
# c:/testweb
# /testweb

```
### date_parse_from_format()
```
//返回给定日期的详细数组
//参数 格式 给定日期
//返回个数组
//array date_parse_from_format ( string $format , string $date )
date_parse_from_format();
```
## F
### filter_var()
```
//函数通过指定的过滤器过滤变量。如果成功，则返回已过滤的数据，如果失败，则返回 false。
//参数 规定要过滤的变量 可选。规定要使用的过滤器的 ID。 规定包含标志/选项的数组。检查每个过滤器可能的标志和选项。 http://www.w3school.com.cn/php/php_ref_filter.asp

//filter_var(variable, filter, options)
filter_var
```
### file_exists()


## G
### get_class()
```
//返回对象的类名
//参数类实例
//返回一个类名字符串
get_class()
```

## I
### ini_set()
```
//设置指定配置选项的值。这个选项会在脚本运行时保持新的值，并在脚本结束时恢复。取值用 ini_get()
//参数 $varname  : 选项名称
//参数 $newvalue : 选项新的值
//返回 成功时返回旧的值，失败时返回 FALSE。
string ini_set ( string $varname , string $newvalue )
```

## M
### method_exists()
```
//检查类的方法是否存在
//参数 前面是类实例 后面是方法名
//返回是个布尔值
method_exists()
```

## P
### property_exists()
```
//检查对象或类是否具有该属性
//参数 类 属性名
//返回 布尔值
bool property_exists ( mixed $class , string $property )
property_exists()
```
### preg_split()
```
//通过一个正则表达式分隔字符串
//参数 搜索模式正则 被搜索的字符串 限度最多被分成几个部分 flags看手册吧讲了很多但不太主要
//返回数组被分割的数组
//array preg_split ( string $pattern , string $subject [, int $limit = -1 [, int $flags = 0 ]] )
preg_split()
```

### property_exists()*

## S
### str_replace() 
```
//该函数子字符串替换
//参数 查询部分 替换部分 字符串
//返回一个字符串或者数组
str_replace() 

#例
echo str_replace("world","Shanghai","Hello world!");
-----
#output
# Hello Shanghai!
```

## U
### unlink()
