---
title: laravel 安装 (windows)
date: 2016-05-29 21:05:10
tags: [php,laravel]
categories: php开发
---
**第一步 安装composer**
 从这里 https://getcomposer.org/download/ 后直接安装即可 切记需先安装php 并且版本支持做开发项目

**第二步 配置composer镜像仓库**
```composer config -g repo.packagist composer https://packagist.phpcomposer.com```
输入这行代码即可 你可以在任意位置打开命令窗口输入 这样就是全局配置 
当如你也可以进入项目根目录 在那里打开命令窗口输入 这样就是那个项目的配置而不是全局的 以后开发项目年还有选择的机会

**第三步 安装laravel**
```composer create-project laravel/laravel myapp --prefer-dist```
使用这句就可以创建新的项目 myapp为指定目录
如果遇到报错 请用 ```composer selfupdate``` 更新一下 然后尝试安装
以上你的laravel项目已经建立完成 那么动手写吧

**必要补充**
你也可以使用laravel安装器 然后通过安装器来建立新的项目
```composer global require "laravel/installer=~1.1"```
使用上面代码安装laravel安装器并确保 *C:\Users\ `你用户名`\AppData\Roaming\Composer\vendor\bin*
或者 *C:\ProgramData\ComposerSetup\bin* 因为我当前机器的系统环境变量是这里 
下面有`laravel.bat`这个文件
接下来你就可以使用
```laravel new blog```来建立项目了 blog为你的项目名

好了 就是这些 开始码代码吧
