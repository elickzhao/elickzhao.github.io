---
title: 关于child_process中的exec和spawn
date: 2017-06-19 21:41:49
tags: [node.js,js,小百科]
categories:
---

> 这是篇转发,因为原文写的实在是太明白了. [http://cnodejs.org/topic/507285c101d0b80148f7c538](http://cnodejs.org/topic/507285c101d0b80148f7c538)

开始学习child_process模块的时候以为spawn可以直接运行命令, 后来发现这是一个小陷阱就拿出来和大家分享一下.

先说下我碰到的情况由于在windos下写的所以根据docs上的例子我就写出了这么一句代码:"require(“child_process”).spawn(“dir”), 这么写是会有错误的,用error接受到的数据是没有此文件. 而用exec就不会有问题,于是得到了以前的猜想.

大家都知道在linux下, ls命令对应的是一个文件, 而在windows下是做为cmd的内置命令的. 所以像我那样写是会报错.

```js
于是我查看了child_process的源码发现spawn是这样定义的var spawn = exports.spawn = function(file, args, options); 也就是说他传入的应该是一个文件, 例如ping, cmd等. 而exec的源码中有一段这样的代码:

 if (process.platform === 'win32') {
file = 'cmd.exe';
args = ['/s', '/c', '"' + command + '"'];
// Make a shallow copy before patching so we don't clobber the user's
// options object.
options = util._extend({}, options);
options.windowsVerbatimArguments = true;
} else {
  file = '/bin/sh';
  args = ['-c', command];
}
```


所以想使用内置命令可以直接使用exec或者把spawn改成spawn(“cmd.exe”,["\s", “\c”, “dir”]);

总结起来就是spawn是调用一个文件! 不要被docs上的child_process.spawn(command, [args], [options])中的command给骗了
