---
title: TP 3.2加载第三方库文件
tags:
  - php
  - 小百科
abbrlink: 49237
date: 2018-01-13 19:10:37
categories:
---


今天用到Carbon这个时间类,不过**TP3.2**不使用 `composer` 所以加载起来比较费劲.
按[官方说法](https://www.kancloud.cn/manual/thinkphp/1701)放在 `/thinkphp/Library` 下的都可以自动加载.
其实呢,需要命名空间与目录一致,而且文件名必须是 `*.class.php` 这大多数第三方库是满足不了的
所以只好手动加载了

```php
<?php
namespace Admin\Controller;

use Think\Controller;
//import("Vendor.Carbon.Carbon");   //这两种加载文件的方法都可以
vendor("Carbon.Carbon");
use Carbon\Carbon;  //使用了 Carbon 命名空间 那么下面使用的时候 就不需要每次都加了
class IndexController extends PublicController
{
    //***********************************
    // iframe式显示菜单和index页
    //**********************************
    public function index()
    {
        
        //这是另一个第三方库 微信支付 
        //不过呢 这个库比较早 没有命名空间
        //所以使用的时候 必须用\空间  也就是 \WxPayUnifiedOrder() 这种形式使用
        //这是需要注意的地方 没有命名空间都得这么使用
        $b = vendor("wxpay.wxpay");
        $input = new \WxPayUnifiedOrder();
        dump($input);

        printf("Now: %s", Carbon::now());   //这里因为上面使用 use 可以这么简写
        
        //如果头部没有用use的话,在这里使用必须写完整的命名空间才行
        vendor("Carbon.Carbon");
        printf("Now: %s", \Carbon\Carbon::now());
        exit();
        .....
    }
}

```