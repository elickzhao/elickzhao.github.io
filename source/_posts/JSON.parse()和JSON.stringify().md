---
title: JSON.parse()和JSON.stringify()
date: 2016-11-25 22:32:11
tags: js
categories:
---
parse用于从一个字符串中解析出json对象,如

    var str = '{"name":"huangxiaojian","age":"23"}'

结果：
```
JSON.parse(str)



Object

age: "23"
name: "huangxiaojian"
__proto__: Object
```


**注意**：单引号写在{}外，每个属性名都必须用双引号，否则会抛出异常。

---

stringify()用于从一个对象解析出字符串，如


    var a = {a:1,b:2}

结果：

```
JSON.stringify(a)


"{"a":1,"b":2}"
```

参考文档
[JSON.parse()和JSON.stringify()](http://blog.csdn.net/wangxiaohu__/article/details/7254598)
[ MDN JSON.parse()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse)
