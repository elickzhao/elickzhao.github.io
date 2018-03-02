---
title: js继承 extend mixin
date: 2017-06-27 23:13:20
tags: [js,node.js]
categories:
---

这两个说有用也有用,说没用也没用,这是因为必须这个类支持这两个方法才能用,如果不支持是用不了的
现在又有个什么`Composition` 不过这个还没来得及看

**mixin**
```js
var _   = require('lodash');
var _db = require('lodash-id');

_.mixin(_db);
```

**extend**
```js
//_应该是lodash 这是个插件或者说类
_.extend(app.locals, require(’./common/render_helpers’));
```

## 参考文档
[ JavaScript-mixin实现多继承](http://blog.csdn.net/qiqingjin/article/details/51762435)
[http://cnodejs.org/topic/5413e4ce8518442c62f3b503](http://cnodejs.org/topic/5413e4ce8518442c62f3b503)
[Mixin 已死，Composition 万岁](http://www.open-open.com/news/view/1245b26)
