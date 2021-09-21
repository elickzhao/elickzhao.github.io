---
title: laravel 简单自定义个artisan命令
tags:
  - laravel
  - php
categories: php开发
abbrlink: 50420
date: 2016-05-24 00:11:59
---

###参考 artisan make:model 而写的 make:view 新建blade模版

- MakeView.php
```php
<?php

namespace App\Console\Commands;

use Illuminate\Console\Command;
use Illuminate\Filesystem\Filesystem;

class MakeView extends Command
{
    /**
     * The name and signature of the console command.
     *
     * @var string
     */
    protected $signature = 'make:view {name : like content or article/content}';

    /**
     * The console command description.
     *
     * @var string
     */
    protected $description = 'Create a new blade page';

    /**
     * The type of class being generated.
     *
     * @var string
     */
    protected $type;

    /**
     * 文件系统
     * @var Filesystem
     */
    protected $files;
    /**
     * Create a new command instance.
     *
     * @return void
     */
    public function __construct(Filesystem $files)
    {
        parent::__construct();

        $this->files=$files;
    }

    /**
     * Execute the console command.
     *
     * @return mixed
     */
    public function handle()
    {
        //
        $path = $this->getPath($this->argument('name'));

        if($this->alreadyExists($path)){
            $this->error($this->type.' already exists!');
            return false;
        }

        $this->makeDir($path);

        $this->files->put($path, $this->getStub());

        return $this->info($this->type.' created successfully.');
    }

    /**
     * Get path
     * @param string $name
     * @return string
     */
    protected function getPath($name){
        return base_path('resources/views')."/".$name.".blade.php";
    }

    /**
     * 模版是否已经存在
     * @param $path
     * @return bool
     */
    protected function alreadyExists($path){
       return $this->files->exists($path);
    }

    /**
     * 建立目录
     * @param $path
     */
    protected function makeDir($path){
        if (! $this->files->isDirectory(dirname($path))) {
            $this->files->makeDirectory(dirname($path), 0777, true, true);
        }
    }

    /**
     * 获得模版内容
     * @param string $stub //现在默认为bootstrap风格 以后还可以添加妹子UI风格模版等等
     * @return string
     * @throws \Illuminate\Contracts\Filesystem\FileNotFoundException
     */
    protected function getStub($stub='view.stub'){
        return $this->files->get(__DIR__.'/stubs/'.$stub);
    }
}

```
<!--more-->

- view.stub
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <meta http-equiv="x-ua-compatible" content="ie=edge">
  <title>New Page</title>
  <link rel="stylesheet" href="//cdn.bootcss.com/bootstrap/3.3.5/css/bootstrap.min.css">
  <script src="//cdn.bootcss.com/jquery/1.11.3/jquery.min.js"></script>
  <script src="//cdn.bootcss.com/bootstrap/3.3.5/js/bootstrap.min.js"></script>
</head>
<body>

</body>
</html>
```

##注意
>想要自定义参数 比如短参数必须使用两个方法 getArguments() 和 getOptions()

###补充
>又看了遍源码 找到更方便的方法 这个文档里没有写 所以下面两种方法都可以
```
//直接设置属性就可以了 切记短参数放在前面用 | 分割
protected $signature = 'make:view {name : like content or article/content} {--l | layout : create new layout}';

//这里还要注意一点是 在Parser.php 里的 $matches = preg_split('/\s*\|\s*/', $token, 2); 中间的 \| 是转义成了 | 字符 不要在忽略 认为是或了 给自己提个醒
```

```php
<?php
/**
 * Created by elick.
 * Class: MakeView
 * Date: 2016/3/16
 * Time: 18:28
 * Description: Brief description
 */

namespace App\Console\Commands;

use Illuminate\Console\Command;
use Illuminate\Filesystem\Filesystem;
//引入相关类
use Symfony\Component\Console\Input\InputArgument;
use Symfony\Component\Console\Input\InputOption;

class MakeView extends Command
{
    //这里特别注意 取消了 $signature 这个属性 使用$name否则下面两个方法都不好使
    protected $name = 'make:view'; 

    /**
     * The console command description.
     *
     * @var string
     */
    protected $description = 'Create a new blade page';

    /**
     * The type of class being generated.
     *
     * @var string
     */
    protected $type = 'View';

    /**
     * 文件系统
     * @var Filesystem
     */
    protected $files;

    /**
     * Create a new command instance.
     *
     * @return void
     */
    public function __construct(Filesystem $files)
    {
        parent::__construct();

        $this->files = $files;
    }

    /**
     * Execute the console command.
     *
     * @return mixed
     */
    public function handle()
    {
        //XXX 还有一点就是这里了 handle和fire有啥区别 有空看看那
        $path = $this->getPath($this->argument('name'));

        if ($this->alreadyExists($path)) {
            $this->error($this->type . ' already exists!');
            return false;
        }

        $this->makeDir($path);

        $content = $this->option('layout') ? $this->getStub() : "";

        $this->files->put($path, $content);

        return $this->info($this->type . ' created successfully.');
    }

    /**
     * Get path
     * @param string $name
     * @return string
     */
    protected function  getPath($name)
    {
        return base_path('resources/views') . "/" . $name . ".blade.php";
    }

    /**
     * 建立目录
     * @param $path
     */
    protected function makeDir($path)
    {
        if (!$this->files->isDirectory(dirname($path))) {
            $this->files->makeDirectory(dirname($path), 0777, true, true);
        }
    }

    /**
     * 获得模版内容
     * @param string $stub //现在默认为bootstrap风格 以后还可以添加妹子UI风格模版等等
     * @return string
     * @throws \Illuminate\Contracts\Filesystem\FileNotFoundException
     */
    protected function getStub($stub = 'view.stub')
    {
        return $this->files->get(__DIR__ . '/stubs/' . $stub);
    }

    /**
     * 模版是否已经存在
     * @param $path
     * @return bool
     */
    protected function alreadyExists($path)
    {
        return $this->files->exists($path);
    }

    protected function getArguments()
    {
        return [
            ['name', InputArgument::REQUIRED, 'The name of the class'],
        ];
    }

    protected function getOptions()
    {
        return [
            ['layout', 'l', InputOption::VALUE_NONE, 'Create a new migration file for the model.'],
        ];
    }
}

```
