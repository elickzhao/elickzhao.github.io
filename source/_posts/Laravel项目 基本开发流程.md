---
title:  Laravel项目 基本开发流程
date: 2016-05-28 22:20:20
tags: [php,laravel]
categories: php开发
---


###用composer建立项目
莫名其妙的5.1.11的库文件少了 ```vendor``` 这个目录所以拿composer无法创建了 只好创建5.2 或者下载一键安装包
```
composer create-project laravel/laravel myapp --prefer-dist
```

###配置项目环境
- 配置数据连接 
    打开.env文件进行配置 (这个必须先行配置 要不下面插件安装会提示错误) 还有就是config里的database.php的配置文件一般是用于多数据库连接时在里面进行修改
- 初始化composer 
    在配置里搜索composer然后把 composer.phar地址填写进去 没有的话 就按照提示下载一个或者安装一下 (C:\ProgramData\ComposerSetup\bin\composer.phar 这是我的位置仅供参考)然后点确定初始化完成
- 配置Command Line Tool Support
    1 点击添加 选择Tool based on Symfony Console 确定
    2 起一个别名 然后添加脚本地址 也就是artisan所在位置 如果php.exe不在运行环境里 那也得选择地址
    3 完成后可以点编辑选择是当前项目下使用 还是全局使用(~~上次我是选择当期项目下 这次新建立项目就又装了一遍 这次我选全局了 看看下次开项目是不是还用再装一遍~~  ~~已经正事了是全局的 第二次不用配置了~~ 看来我又错了 虽然为全局的不用安装 但是命令还在那个命名空间 所以生成的文件还在老的项目里 而不能生成在新的项目所以没用)
    4 配置文件可以复制进去 但是必须替换里面的项目名 要不会报错
##注意
Command Line Tool Support 在laravel 5.2里配置artisan会报错 具体原因不明 也许是我的phpstorm版本的问题 我这个一直没升级 还是10.0.1  其实命令是好使的在cmd里 只不过还得输入那么多 所以项目降级到laravel 5.1了 这次遇到的麻烦太多了 5.1还不能用composer生成线上的库不知道什么问题缺少vendor 所以只能用一键安装包来新建项目 而且5.2插件支持也变了所以原本打算用5.2最后也是放弃了

<!--more-->

###给phpstrom安装插件
- Laravel plugin
```
//phpstrom自带插件在配置里的plugins里搜索Laravel就开会出现了  (这个能提示大多数代码)
//注意有个问题 虽然可以不用重新安装 但是没打开一个新的项目就得重新激活一下
// 设置 -> 其他设置 -> laravel plugin -> Enable plugin for this project
//这样这个插件才好使
```
- Laravel-ide-helper 
```
//安装程序
composer require barryvdh/laravel-ide-helper
//配置文件到 config > app.php > 服务数组里
Barryvdh\LaravelIdeHelper\IdeHelperServiceProvider::class,
//生成提示文件 这样一些静态命令才会有提示
php artisan ide-helper:generate
//在composer.json里 添加更新时命令 (为确保提示为最新的 当加载程序变更时 自动重启生成 顺序不能变)
        "pre-update-cmd": [
            "php artisan clear-compiled",
            "php artisan ide-helper:generate",
            "php artisan optimize"
        ],
```

- illuminate/html 
这个包在laravel 5.2下换名了 所以不能用下面那个地址了 改成了 ```"laravelcollective/html": "5.2.*"``` 包的位置也换成了 ```Collective\Html\```
```
//安装程序
composer require illuminate/html:^5.0
//配置程序
Illuminate\Html\HtmlServiceProvider::class,
//设置门面别名 也就是app.php配置文件里的aliases数组里 感觉设置一个就行了 两个都是相同的
'Form'      => Illuminate\Html\FormFacade::class,
'Html'      => Illuminate\Html\HtmlFacade::class,

/*
在模版文件里From::不出提示 
那是因为直接用了composer require这样直接安装 而不是使用写在composer.json里用 update安装 所以ide-helper命令没有执行
解决办法:手动update一下,或者ide-helper命令一下 提示就会出现了
还有个一劳永逸的方法就是 把这个命令再放到 "pre-install-cmd" 数组里 这样安装的时候也自动执行了
*/

```
- Laravel Debugbar
```
//安装程序
composer require barryvdh/laravel-debugbar
//配置程序
Barryvdh\Debugbar\ServiceProvider::class,
//配置门面别名
'Debugbar' => Barryvdh\Debugbar\Facade::class,
```

- 配置Command Line Tool Support 
[官方参考文档](https://confluence.jetbrains.com/display/PhpStorm/Command+Line+Tools)
```
    #上面已经配置好了 就可以直接用了 使用Ctrl+Shift+X调用Command Line工具 在里面直接输入artisan命令 可以提示和补全 很方便
    #还有个问题是自定义命令的话 是不会加到提示里的比如我自己加的make:view(已经确定可以加提示) 但是可以自己手动添加提示 就在配置页面的open definition on editor这个选项 就是右侧第四个 点开这个就会出现一个xml文件 按着格式把命令填进去后面就可以使用了
    #以后也许可以写个程序自动添加进去
```

###建立数据库
数据库已经在上面环境配置建立完毕,现在开始建立所需要的表

- 建立模型
```
//用命令直接建立模型和数据库迁移
artisan make:model Model/Task -m

//修改迁移文件
#2016_03_17_085421_create_tasks_table.php
    public function up()
    {
        Schema::create('tasks', function (Blueprint $table) {
            $table->increments('id');
            $table->string('name'); //增加个字段name
            $table->timestamps();
        });
    }

//执行迁移命令
artisan migrate
```

- 编写路由
- 编写模版
