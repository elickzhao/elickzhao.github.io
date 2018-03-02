---
title: laravel 银联支付一点总结
date: 2016-05-15 22:08:05
tags: laravel
categories: php开发
---

- 第一步开通测试
 >在我的产品中 点击为测试 然后添加要测试的项目 要不然会提示无权限 这里选择网关支付

- 下载测试证书
>测试证书要从 个人中心 测试参数哪里下载  https://open.unionpay.com/ajweb/account/testPara
  商户私钥证书 必须安装一下 要不会会出错

- 编写代码
1. config\laravel-omnipay.php
```php
<?php

return [

	// The default gateway to use
	'default' => 'paypal',

	// Add in each gateway here
	'gateways' => [
		'unionpay' => [
			'driver'  => 'UnionPay_Express',
			'options' => [
				'merId'			 => '777290058128659',
				'certPath'		 =>	storage_path('app/unionpay/certs/700000000000001_acp.pfx'),
				'certPassword'	 => '000000',
				'certDir'		 =>	 storage_path('app/unionpay/certs'),
				//付款完成后跳转会商家页面地址 两个好像可以写同一个地址
				'returnUrl' => 'http://114.254.183.127/omnipay/public/unionpay/return',
				'notifyUrl' => 'http://114.254.183.127/omnipay/public/unionpay/notify'

			]
		]
	]

];
```

<!--more-->

2. routes.php
>回调的方式不要写错了 是 **POST**   
```php
//银联支付处理
Route::get('unionpay/pay','UnionpayController@pay');
//支付后回调页面
Route::post('unionpay/return','UnionpayController@result');
```

3. UnionpayController.php
>注意引用的 一定是 Omnipay的门面 因为会自动加载 Omnipay\Omnipay这个类 这是错的 会找不到gateway()方法的
```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

use App\Http\Requests;
use App\Http\Controllers\Controller;
use Omnipay;     //****************  就是这里千万不要搞错 引用的是这个门面

//现在返回还有点问题 但是无关大雅了 支付流程已经完成了 
class UnionpayController extends Controller
{
    public function pay(){
        $gateway = Omnipay::gateway('unionpay');

        $order = [
            'orderId'   =>  date('YmdHis'),
            'txnTime'   =>  date('YmdHis'),
            'orderDesc' =>  'My test order title',
            'txnAmt'    =>  '100'   //订单价格
        ];
//        dump(config('laravel-omnipay.gateways'));
//        dd(storage_path('app/unionpay/certs/acp_test_sign.pfx'));
        $response = $gateway->purchase($order)->send();
        $response->redirect();

    }

    public function result(){
        $gateway = Omnipay::getFactory('unionpay');
        $response = $gateway->completePurchase(['request_params'=>$_REQUEST])->send();
        if ($response->isPaid()) {
            exit('支付成功！');
        }else{
            exit('支付失败！');
        }
    }

    public function  notify(){
        echo 'hhhh';
    }
}

```
4. 测试同调
>有一点一定要记住  **点击获取短信** 后,再填写验证码 要不有可能会提示验证码错误,或者系统忙