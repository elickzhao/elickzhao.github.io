---
title: module.exports与exports，export与export default解析
date: 2017-07-08 21:13:07
tags: [JavaScript,node.js]
categories:
---

其实下面文章写的特别好了,真没必要在复制一份,忘记的时候就再看一下下面的文章吧

下面写点自己容易忽视的东西吧.

`module.exports` `exports` 与 `export` `export default` 注意哟 少个 S

`module.exports` 是 **CommonJS模块规范** `export` 是 node 的一个简写,不用你写那么多了

`export` `export default` 是 **ES6模块规范** `export default`就是为模块指定默认输出

```js
// module.exports 例子
var x = 5;
var addX = function (value) {
  return value + x;
};
module.exports.x = x;
module.exports.addX = addX;

------------------------------------------------------------------

var example = require('./example.js');

console.log(example.x); // 5
console.log(example.addX(1)); // 6

------------------------------------------------------------------

//export 例子

var firstName = 'Michael';
var lastName = 'Jackson';
var year = 1958;

export {firstName, lastName, year};

export default function () {
  console.log('foo');
}

------------------------------------------------------------------

//ES6 有这个优势吧,可以分开引入部分内容 exports 可能就不行吧 .我是这么觉得的
import { firstName, lastName } from 'demo1'


```

## 参考文档
这个文章很好,讲的很详细
[module.exports与exports，export与export default之间的关系和区别](http://www.cnblogs.com/fayin/p/6831071.html)
