---
title: windows 下安装VirtualBox , Vagrant 和 Homestead
date: 2016-05-04 18:54:01
tags: [vagrant,服务器相关技术,laravel]
categories:
---

- 下载 VirtualBox 并安装 这个不用说一顿下一步就好了 [下载地址](https://www.virtualbox.org/wiki/Downloads)

- 下载安装 Vagrant 这个也是跟上面一样 [下载地址](https://www.vagrantup.com/) 哦 记得重启电脑要不命令不好使的

- 下载 Homestead Vagrant Box  这里你可以使用命令 `vagrant box add laravel/homestead`  慢慢等 大概20分钟左右吧 `(使用命令时会让你选择 记得要选择 virtualbox 如果你装的VM那就选另一个就好了)`
也可以看到下载地址自己下 就是在输入命令后 开始下载时 果断 Ctrl+C ![就是这里](https://dn-phphub.qbox.me/uploads/images/201502/22/1/MK7b6C67N1.png)
通过地址我发现 [直接去浏览版本](https://atlas.hashicorp.com/laravel/boxes/homestead) 然后进入版本后在后面 接上这个 `providers/virtualbox.box` 就可以下载了
>我试了下下载速度其实快不了多少 但是有个好处可以断点续传 而使用命令是不可以的 如果失败了 是比较麻烦的
https://atlas.hashicorp.com/laravel/boxes/homestead/versions/0.4.2/providers/virtualbox.box

**失败了...  唉下载那种方式我失败了 总说文件无法打开 也不知道为啥 但是别人都成功了 算了不管了 以后再说吧 还是直接用命令下 反正时间也没差多少**
<!--more-->

- 命令执行后 会在 C:\Users\\ `你的用户名` \\.vagrant.d\boxes 下面增加这个box (名字是laravel-VAGRANTSLASH-homestead) 如果想删除可以直接上这里删除 (而且上次操作时不知道为什么出来两个版本 一个0.4.2 一个0.3.0 不知道是不是跟我建立了两个虚拟机有关 以后测试下) 不过现在光有box 还没有建立虚拟机

- 下面是下载 laravel/Homestead命令 `composer global require "laravel/homestead=~2.0"` 如果你没装composer 可以直接用git clone 来下载 `git clone https://github.com/laravel/homestead.git Homestead` 可以任选其一 

- 初始化 配置文件 Homestead.yaml 这个有很三个方法 
    1.  去克隆下来文件夹,找到init.bat点击执行就可以了 就会生成默认配置文件 (composer global 下载的文件位置在 C:\Users\ `你的用户名` \AppData\Roaming\Composer\vendor\laravel\homestead)
    2.  还是去到克隆的那个文件夹 使用 bash init.sh 来生成 但是这个需要你安装Git 并且把Git下的bin目录放到环境变量里才可以 否则找不到这个bash命令的
    3.  使用homestead init 来生成默认配置 但是同样 需要配置环境变量 否则无法使用, 建议还是把composer配置到环境变量,因为其他一些项目有时也需要执行一些命令 这样可以一劳永逸 比如laravel-install (配置环境变量的目录 C:\Users\ `你的用户名`\AppData\Roaming\Composer\vendor\bin) 
**~~第三个方法又失败了 可能windows下不能使用 只有在linux才能使用 反正提示没有init这个命令 而且 -h 也没有 找到这个命令~~**  可以了原来是 laravel/Homestea 版本问题 如果是3.0版本就没有这个命令 2.0的版本就有这个命令 

- 好了 这种方式是全局安装 接下来就是配置这个 主机的配置了 
>这种方式是全局安装，即同一主机上所有项目共享该Homestead盒子，当然你也可以为每个项目单独指定Homestead盒子，可参考[Laravel Homestead相应的文档](http://laravelacademy.org/post/51.html#ipt_kb_toc_51_6)，这里不再赘述。

```
---
# 虚拟主机ip 这个默认就可以不需要改
ip: "192.168.10.10"

# 虚拟主机 使用内存
memory: 2048

# 虚拟主机 使用CPU核心数
cpus: 1

# 使用哪个 Vagrant 提供者： virtualbox 、 vmware_fushion 或者 vmware_workstation 我们下载的是virtualbox版 所以默认就是这个了
provider: virtualbox

# 私有SSH KEY 这个我没有 也能登录和使用
authorize: ~/.ssh/id_rsa.pub

keys:
    - ~/.ssh/id_rsa
# 共享文件夹
folders:
    - map: F:\phpStudy\WWW\homestead
      to: /home/vagrant
    - map: F:\phpStudy\WWW\homestead\laravelapp
      to: /home/vagrant/laravelapp  
      
# 站点配置 这个需要配置本地host
sites:
    - map: homestead.app
      to: /home/vagrant
    - map: laravel.app
      to: /home/vagrant/laravelapp/public
      //hhvm: true  这个必须对齐to 要不会报错
      
#多站点可以这么写 不过记住添加新站点一定要执行 vagrant provision 命令
#否则nginx里不会有新站点配置 域名不会指向 当然要手动自己写也可以
      
# 数据库名
databases:
    - homestead

# 数据库对应.env环境变量 也就是laravel项目.env配置文件
variables:
    - key: APP_ENV
      value: local

#黑火测试配置
# blackfire:
#     - id: foo
#       token: bar
#       client-id: foo
#       client-token: bar

#端口映射 可以把虚拟机端口
# ports:
#     - send: 93000
#       to: 9300
#     - send: 7777
#       to: 777
#       protocol: udp
#测试一则 这里必须有空格 方向别搞反了
ports:
    - send: 8080
      to: 80

```

- 启动并初始化 homestead 虚拟机 进入自己的目录 使用命令  `Homestead up`  **这个命令是2.0版本才有 3.0版本是没有的 而且使用前必须使用homestead init 要不即使有配置文件也会出错 还有个问题就是这个版本会找0.3.0那个版本的box 也就是说我上面下载的0.4.2那个没用了 也就是为什么上面说会出现两个版本 看来还得用新的3.0 Homestead试一下**

- **有几个问题需要注意 共享目录如果配置错误 更改配置文件 居然也不好使 只能销毁重新建立虚拟机 不知道为什么 其他设置均可以 可能我这个机器Vagrant现在也有些问题 而且还有个问题共享目录php文件不显示 只显示laravel项目 很诡异 我在根目录放歌index.php文件就不显示 但是laravel项目指定域名就可以**

- 这个新版本的3.0 很是麻烦 去掉了很多命令 就剩了一个make  ~~全局的虚拟机好像不好使了~~ 只有项目的虚拟机可以用 而且比较简单 只要在项目里 `composer require laravel/homestead` 并在项目根目录执行 `homestead make`  然后启动就可以了 `vagrant up` 
- 全局的虚拟机可以使用 步骤是
    1. 先安装 VirtualBox 和 Vagrant 
    2. 到用户目录 也就是进入 `C:\Users\elick` 以后 `git clone https://github.com/laravel/homestead.git Homestead` 最好不要用composer 用Git吧 这个比较方便 和上个版本不一样了
    3. 进入 Homestead 用命令生成配置文件 `bash init.sh` 或者点击init.bat 这样生成C:\Users\ `你的用户名` \.homestead 目录并且在目录下生成配置文件
    4. 然后这步很关键了 进入刚才git clone下来的 Homestead 目录 在这个目录下 vagrant up 其他目录使用这个命令是没用的 只有在这个目录里使用才可以 这样就生成了全局虚拟机
    5. 几个有用的命令
```
vagrant destroy --force     //删除命令
vagrant reload --provision  //更新配置文件 好像说是更新网站配置文件
```

**注意:**  `vagrant up` 必须在 Homestead 目录下使用,还有就是直接启动虚拟机的话,那么 `Homestead.yaml`的配置就会失效,所以必须用 `vagrant up` 来启动虚拟机. 

***说明:** 其实主要是挂载共享目录问题,端口是没问题的,只不过是默认端口,不是你配置的那个.所以你需要进入虚拟机后 手动挂载共享目录,这样也可以用的,只不过何必跟自己找麻烦,还是老实用`vagrant up`吧

- 一些文章地址
1. [windows 安装laravel Homestead](http://blog.csdn.net/small_rice_/article/details/45366299)
2. [vagrant打造自己的开发环境~~我也来一发](http://lovelace.blog.51cto.com/1028430/1423343)
3. [1+1>2:用Docker和Vagrant构建简洁高效开发环境](http://cloud.51cto.com/art/201503/470256_all.htm)
4. [{Laravel 5.2 文档}  开始 —— Laravel Homestead](http://laravelacademy.org/post/2749.html)
5. [Laravel Homestead](https://laravel.com/docs/5.2/homestead)  学院翻译的不完全 下面半段没翻译 所以还得看原版