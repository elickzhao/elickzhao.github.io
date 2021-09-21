---
title: node.js 自建简易服务
tags:
  - node.js
  - 前端
abbrlink: 3112
date: 2017-06-14 23:35:18
categories:
---


废话先不多说,代码就是如下这么简单.

```js
var express = require('express')
var app = express()

app.use(express.static('./dist')) //指定目录

app.get('/', function (req, res) {
  res.send('Hello World')
})

app.listen(3000)

```

下面这个是参考文档里的写法
```js
var http = require('http');
var express = require('express');
var app = express();
app.use("/public", express.static(__dirname + '/public'));  //访问路径写到一起了

// 创建服务端
http.createServer(app).listen('80', function() {
	console.log('启动服务器完成');
});
```

然后执行 `node app.js` 就可以启动服务器了. app.js 就是上面配置所保存的文件名.

<!--more-->

上面用的是`express`,用`http-server`的话就更简单,不需要自己写配置直接用命令就可以启动了.
不过这个需要全局安装才行,要不不会有命令的.

```js
//如果你的当前项目中存在 public 文件夹,那么默认静态目录会指定到 public
//如果没有 public 文件夹,那么静态目录就是 根目录
//所以要哪个目录充当静态服务器的根目录 就得进入哪个目录执行下面命令
http-server -a 127.0.0.1 -p 7070  
```


**下面开始唠叨:**

因为现在做动静分离的后台程序,所以需要前端的静态服务器.在本地时单击打开静态文件,有时因为所需插件原因.
也是无法打开的,必须放在静态服务器.所以这时就需要上面的东西了.
关于设置静态服务器的必要性 看看这里 [是否有必要为网站的静态资源设置一个单独的服务器?](https://segmentfault.com/q/1010000004050694?_ea=470962)能了解到不少东西.

这两个搭建服务器的插件 `express`和 `http-server` 都得先安装才能用.不过呢 因为项目里有时用到别的插件.
比如 `webpace`的时候 `express`就不用特别安装了.因为已经包涵在里面.但是`http-server`使用的范围不是那么广,所以必须安装.

还有就是下面参考文档里说,`http-server`比`express`要小巧,不过从下载的包来看,并不是如此.可能指的的是功能上吧. 而且`express`可以操作数据库,这可能对我要写的程序有点用处.


##参考文档
[Node.js用6行代码1个JS文件搭建一个HTTP静态服务器](https://my.oschina.net/obullxl/blog/163049)
[随笔 http-server 快速创建node.js 静态服务器](http://yijiebuyi.com/blog/b0f6ddc56be457e13879a3ad105f561b.html)
[http-server Angular.js 后端node服务首选 轻量级替换 Express 解决方案](http://yijiebuyi.com/blog/359ff66c69934c178dfa8baa32427aef.html)
