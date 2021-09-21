---
title: ES6 新函数 Promise 简介
tags:
  - js
  - 小百科
abbrlink: 34787
date: 2017-06-23 22:38:40
categories:
---


这就是Promise的作用了，简单来讲，就是能把原来的回调写法分离出来，在异步操作执行完后，用链式调用的方式执行回调函数。

```js
function runAsync(){
    var p = new Promise(function(resolve, reject){
        //做一些异步操作
        setTimeout(function(){
            console.log('执行完成');
            //执行成功后 执行这个回调
            resolve('随便什么数据');
            //reject是失败
        }, 2000);
    });
    return p;            
}
runAsync().then(function(data){
    console.log(data+'-------');
    //后面可以用传过来的数据做些其他操作
    //......
});
```

功能相同

```js
function runAsync(callback){
    setTimeout(function(){
        console.log('执行完成');
        callback('随便什么数据');
    }, 2000);
}

runAsync(function(data){
    console.log(data);
});
```

**优点:**

效果也是一样的，还费劲用Promise干嘛。那么问题来了，有多层回调该怎么办？如果callback也是一个异步操作，而且执行完后也需要有相应的回调函数，该怎么办呢？总不能再定义一个callback2，然后给callback传进去吧。而Promise的优势在于，可以在then方法中继续写Promise对象并返回，然后继续调用then来进行回调操作。

<!--more-->

```js
runAsync1()
.then(function(data){
    console.log(data);
    return runAsync2();
})
.then(function(data){
    console.log(data);
    return runAsync3();
})
.then(function(data){
    console.log(data);
});
```

**reject的用法**


```js
function getNumber(){
    var p = new Promise(function(resolve, reject){
        //做一些异步操作
        setTimeout(function(){
            var num = Math.ceil(Math.random()*10); //生成1-10的随机数
            if(num<=5){
                resolve(num);
            }
            else{
                reject('数字太大了');
            }
        }, 2000);
    });
    return p;            
}

getNumber()
.then(
    function(data){
        console.log('resolved');
        console.log(data);
    },
    function(reason, data){
        console.log('rejected');
        console.log(reason);
    }
);
```
**catch的用法**
```js
getNumber()
.then(function(data){
    console.log('resolved');
    console.log(data);
    console.log(somedata); //此处的somedata未定义
})
.catch(function(reason){
    console.log('rejected');
    console.log(reason);
});
```
**注意:**
效果和写在then的第二个参数里面一样。不过它还有另外一个作用：在执行resolve的回调（也就是上面then中的第一个参数）时，如果抛出异常了（代码出错了），那么并不会报错卡死js，而是会进到这个catch方法中。


**all的用法**
Promise的all方法提供了并行执行异步操作的能力，并且在所有异步操作执行完后才执行回调
```js
function runAsync1(){
    var p = new Promise(function(resolve, reject){
        //做一些异步操作
        setTimeout(function(){
            console.log('第一个执行完成');
            //执行成功后 执行这个回调
            resolve('随便什么数据');
            //reject是失败
        }, 2000);
    });
    return p;            
}
function runAsync2(){
    var p = new Promise(function(resolve, reject){
        //做一些异步操作
        setTimeout(function(){
            console.log('第二个执行完成');
            //执行成功后 执行这个回调
            resolve('随便什么数据');
            //reject是失败
        }, 2000);
    });
    return p;            
}
function runAsync3(){
    var p = new Promise(function(resolve, reject){
        //做一些异步操作
        setTimeout(function(){
            console.log('第三个执行完成');
            //执行成功后 执行这个回调
            resolve('随便什么数据');
            //reject是失败
        }, 2000);
    });
    return p;            
}
Promise
    .all([runAsync1(), runAsync2(), runAsync3()])
    .then(function(results){
    console.log(results);
});
```

**race的用法**
all方法的效果实际上是「谁跑的慢，以谁为准执行回调」，那么相对的就有另一个方法「谁跑的快，以谁为准执行回调」，这就是race方法，这个词本来就是赛跑的意思。

**上面例子的测试**

<iframe width="100%" height="450" src="https://code.hcharts.cn/test123/MI4nkz/share/result,js" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

## 参考文档
**这篇文章好棒**
[Javascript 中的神器——Promise](http://www.jianshu.com/p/063f7e490e9a)
[大白话讲解Promise（一）](http://www.cnblogs.com/lvdabao/p/es6-promise-1.html)
