---
title: laravel模型学习笔记
date: 2016-06-04 22:55:09
tags: [laravel,php]
categories: php开发
---


**模型类一些私有属性**
```php
    //指定表名  
    protected $table = 'my_flights';
    
    //这个限制只决定怎么插入到数据库 不决定怎么取出数据
    protected $dateFormat = 'Y-m-d';
    
    //白名单 可以直接创建数据的字段
    protected $fillable = ['title','intro','content','published_at'];
    
    //黑名单 除此之外的字段都可以直接创建
    protected $guarded = ['created_at','updated_at'];
    
    //设置字段为Carbon实例 可以直接使用Carbon方法
    protected $dates = ['published_at'];
    
    //属性转换类型 key是字段名称 value是要转换成的类型
    protected $casts = ['is_admin' => 'boolean',];
    
    //数组转换 把数组转化成JSON格式存入数据库 读取时自动转化成数组
    protected $casts = ['options' => 'array', ];
    
    //隐藏模型的一些属性 直接输出的时候是无法看见的
    protected $hidden = ['password'];
    
    //显示白名单 那些字段直接输出是可以被看到的
    protected $visible = ['first_name', 'last_name'];
    
    //追加字段到返回数组中 而且是数据库没有的字段 而且需要访问器的帮忙
    //但这个不理解有什么用处 他其实是通过已有字段经过判断后输出 两个字段都能返回 只不过这个返回是布尔值
    protected $appends = ['is_admin'];
```
<!--more--> 
    
**数据库一些用法**
```php
App\Flight::updateOrCreate(['name'=>'Flight 10'],['airline'=>'888']);
```

>如果有就更新第一条搜索到的数据 第一个参数搜索条件 第二个参数更新内容 如果没有就创建

>这个方法和*firstOrNew*还有*firstOrCreate* 是一样的这不过这两个只有一个参数 他们用法其实不太很有用查到后只会更新下时间 并没有其他用,如果参数为数组为多元,那么只是加了搜索条件而已搜出来的 还是新的,所以还会接着创建.还有一点Create还得必须修改模型fillable或者$guarded 而New还得用save存一下不能直接插入数据库

-----------------
```php
            DB::table('users')
            ->whereExists(function ($query) {
                $query->select(DB::raw(1))
                    ->from('orders')
                    ->whereRaw('orders.user_id = users.id');
            })
            ->get();
```
>   select * from users where exists (select 1 from orders where orders.user_id = users.id)
    生成上面那句语句  exists 判断括号内语句是否为真  为真则搜索 为假则放弃

------------

**表关联**
>还有个问题就是在函数命名上,也是有规矩的 单数用单数 复数用复数 下面就是例子
```php
    public function account(){
        return $this->hasOne('App\Models\UserAccount');
    }
    
    //如果这里用单数post就会报错 
     public function posts(){
        return $this->hasMany('App\Models\Post');
    }
```

 1. 一对一
Flight表
```php
    public function fly(){
        //外键一般用当前模型的表名加ID 例如 flight_id
        //这个外键是int类型或者是varchar类型都可以
        //第三个参数是表内关联的键  也是就是当期模型的表所含的字段  外键是关联外部的表所含的字段
        return $this->hasOne('App\Flys','flight_fid','airline');
    }
```
Fly表
```php
    >    function flight()
    {
        //默认外键为flight_id  这里的外键还是相对于Flight来说的  这是因为这个是belongsTO从属表  所以外键是位于表内字段
        //return $this->belongsTo('App\Flight','f_id','airline');
        //这里内键也是相对Flight说 其实是Flight的内部字段
        return $this->belongsTo('App\Flight','flight_fid','airline');

    }
```

2. 一对多
```php
    /*
      实际上在底层无论是hasOne方法还是belongsTo方法都可以接收额外参数，
      比如如果user_accounts中关联users的外键是$foreign_key，该外键对应users表中的列是$local_key，
      那么我们可以这样调用hasOne方法：

      $this->hasOne('App\Models\UserAccount',$foreign_key,$local_key);
      调用belongsTo方法也是一样：

      $this->belongsTo('App\User',$foreign_key,$local_key);
      此外，belongsTo还接收一个额外参数$relation，用于指定关联关系名称，
      其默认值为调用belongsTo的方法名，这里是user。
     * */
```

3. 多对多
```php
        //注意我们定义中间表的时候没有在结尾加s并且命名规则是按照字母表顺序，
        //将role放在前面，user放在后面，并且用_分隔
        //所以RoleUser这个model必须指定表名 要不会出错的 protected $table = 'role_user';
        //至于字段就没有说道了
        $user = User::find(1);
```
