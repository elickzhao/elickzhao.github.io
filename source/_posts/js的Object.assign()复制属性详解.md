---
title: js的Object.assign()复制属性详解
tags:
  - js
  - 小百科
abbrlink: 61807
date: 2018-01-08 23:39:33
categories:
---

## 简单描述
**Object.assign()** 方法用于将所有可枚举属性的值从一个或多个源对象复制到目标对象。它将返回目标对象。

```js
Object.assign(target, ...sources)
//target 目标对象
//...sources 源对象
//返回 目标对象
```

**描述:**
>如果目标对象中的属性具有相同的键，则属性将被源中的属性覆盖。后来的源的属性将类似地覆盖早先的属性。

>Object.assign 方法只会拷贝源对象自身的并且可枚举的属性到目标对象。该方法使用源对象的[[Get]]和目标对象的[[Set]]，所以它会调用相关 getter 和 setter。因此，它分配属性，而不仅仅是复制或定义新的属性。如果合并源包含getter，这可能使其不适合将新属性合并到原型中。为了将属性定义（包括其可枚举性）复制到原型，应使用Object.getOwnPropertyDescriptor()和Object.defineProperty() 。

>String类型和 Symbol 类型的属性都会被拷贝。

>在出现错误的情况下，例如，如果属性不可写，会引发TypeError，如果在引发错误之前添加了任何属性，则可以更改target对象。

>注意，Object.assign 会跳过那些值为 null 或 undefined 的源对象。

<!--more-->

## 实例

**简单实例**
```js
var obj = { a: 1 };
var copy = Object.assign({}, obj);
console.log(copy); // { a: 1 }
```

**深拷贝问题**
针对深拷贝，需要使用其他方法，因为 Object.assign()拷贝的是属性值。假如源对象的属性值是一个指向对象的引用，它也只拷贝那个引用值。
```js
function test() {
  'use strict';

  let obj1 = { a: 0 , b: { c: 0}};
  let obj2 = Object.assign({}, obj1);
  console.log(JSON.stringify(obj2)); // { a: 0, b: { c: 0}}
  
  obj1.a = 1;
  console.log(JSON.stringify(obj1)); // { a: 1, b: { c: 0}}
  console.log(JSON.stringify(obj2)); // { a: 0, b: { c: 0}}
  
  obj2.a = 2;
  console.log(JSON.stringify(obj1)); // { a: 1, b: { c: 0}}
  console.log(JSON.stringify(obj2)); // { a: 2, b: { c: 0}}
  
  obj2.b.c = 3; //主要问题在这里 从这里看出对象是个引用 而不同于其他属性 所以不是深度拷贝
  console.log(JSON.stringify(obj1)); // { a: 1, b: { c: 3}}
  console.log(JSON.stringify(obj2)); // { a: 2, b: { c: 3}}
  
  // Deep Clone
  obj1 = { a: 0 , b: { c: 0}};
  let obj3 = JSON.parse(JSON.stringify(obj1));  //这是深度拷贝 也就是个完全复制 建立个新的
  obj1.a = 4;
  obj1.b.c = 4;
  console.log(JSON.stringify(obj3)); // { a: 0, b: { c: 0}}
}

test();
```

**合并对象**
```js
var o1 = { a: 1 };
var o2 = { b: 2 };
var o3 = { c: 3 };

var obj = Object.assign(o1, o2, o3);    //这样使用的话 是会把第一个参数当成目标对象了
console.log(obj); // { a: 1, b: 2, c: 3 }
console.log(o1);  // { a: 1, b: 2, c: 3 }, 注意目标对象自身也会改变。
console.log(o2);  // {b: 2 }, 只是第一个改变了
console.log(o3);  // {c: 3 }, 只是第一个改变了
```


**合并具有相同属性的对象**
```js
var o1 = { a: 1, b: 1, c: 1 };
var o2 = { b: 2, c: 2 };
var o3 = { c: 3 };

var obj = Object.assign({}, o1, o2, o3);
console.log(obj); // { a: 1, b: 2, c: 3 }
```

**更多内容参考下面 MDN文档吧 那里实在是太全了 没必要都复制一遍了**

## 参考文档
[MDN Object.assign()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)
[javascript之Object.assign()痛点](http://blog.csdn.net/waiterwaiter/article/details/50267787)
