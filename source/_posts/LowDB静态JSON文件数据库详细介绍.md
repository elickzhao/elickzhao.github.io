---
title: LowDB静态JSON文件数据库详细介绍
tags: node.js
abbrlink: 32077
date: 2017-07-09 22:12:51
categories:
---

## [lowdb](https://github.com/typicode/lowdb)
>lowdb是一个基于lodash API的轻量级本地JSON数据库  (支持 Node.js , 浏览器 和 Electron)

**特点**

- Small（轻量级）
- Serverless（不需要服务器）
- lodash rich API（lodash丰富的API）
- In-memory or disk-based（基于内存和硬盘的存储）
- Hackable (mixins, id, encryption, ...)(可以扩展第三方插件,比如lodash-id)

## 安装
>npm install lowdb --save
或者
yarn add lowdb

也可以外部引用使用
```js
<script src="https://unpkg.com/lodash@4/lodash.min.js"></script>
<script src="https://unpkg.com/lowdb/dist/lowdb.min.js"></script>
<script>
  var db = low('db')
</script>
```

##使用

<!--more-->

**low()**
```js
//lowdb几种模式
// 内存模式
low()

// 使用异步方式处理本地数据存储
low('db.json', { storage: require('lowdb/lib/storages/file-async') })

// 使用用户自定义数据
low('some-source', { storage: require('./my-custom-storage') })

// 只读模式
const fileSync = require('lowdb/lib/storages/file-sync')
low('db.json', {
  storage: {
    read: fileSync.read
  }
})

// 只写模式
low('db.json', {
  storage: {
    write: fileSync.write
  }
})

```

**db._**
>一个数据库`lodash`实例,用于扩展第三方插件或自定义方法
```js
db._.mixin({
  second: function(array) {
    return array[1]
  }
})

const post1 = db.get('posts').first().value()
const post2 = db.get('posts').second().value()
```

**db.getState()**
>获得数据库状态和内容 是个字符串
```js
db.getState() // { posts: [ ... ] }

//---------
//这么用
fs.writeFileSync('db.json', JSON.stringify(db.getState()))  //fs文件操作
```

**db.setState(newState)**  
>清空数据库

```js
    const newState = {}
    db.setState(newState)
```    

**db.write([source])
db.read([source])**
>这两个都是返回一个promise执行异步操作,可以用于备份

```js
//read和这个完全一样
const db = low('db.json')
db.write()            // writes to db.json
db.write('copy.json') // writes to copy.json
```

**db.defaults()**
>初始化一个数据库
```js
db.defaults({ posts: [], user: {} })
  .write()
```

**.id**
>自动插入id字段,生成一个随机值
```js
db.get('posts').insert({ title: '今天是个好日子!' }).write().id
```


## 常用写法
>根据lodash API 写出的常用写法

**是否存在数据**
```js
db.has('posts')
  .value()
```

**设置数据**
```js
db.set('posts', [])
  .write()
```

**排序前五**
```js
db.get('posts')
  .filter({published: true})
  .sortBy('views')
  .take(5)
  .value()
```
**获得标题**
```js
db.get('posts')
  .map('title')
  .value()
```
**获得数量**
```js
db.get('posts')
  .size()
  .value()
```
**获得第一个标题**
```js
db.get('posts[0].title')
  .value()
```

**更新数据**
```js
db.get('posts')
  .find({ title: 'low!' })
  .assign({ title: 'hi!'})
  .write()
```

**删除数据**
```js
db.get('posts')
  .remove({ title: 'low!' })
  .write()
```

**删除一个属性**

```js
//db.unset('user').write() 可以把这个属性整个删除掉
db.unset('user.name')
  .write()

/* db是这个样子的
{
  "posts": [
    {
      "title": "今天是个好日子!",
      "id": "6dde9a48-e204-401c-b869-96e16605269f"
    }
  ],
  "user": {
    "age": "25",
    "name": "elick"
  }
}  
*/

```

**复制可以用作备份时使用**
```js
db.get('posts')
  .cloneDeep()
  .value()


//---------------------
//这样可以达到备份单个表用途

let clone = db.get('posts')
  .cloneDeep()
  .value()

const db1 = low('copy.json')
db1.defaults({posts:clone}).write()  

```

##添加扩展

**lodash-id**
```js
const db = low('db.json')

db._.mixin(require('lodash-id'))

const postId = db.get('posts').insert({ title: 'low!' }).write().id
const post = db.get('posts').getById(postId).value()
```

**uuid**
```js
const uuid = require('uuid')

const postId = db.get('posts').push({ id: uuid(), title: 'low!' }).write().id
const post = db.get('posts').find({ id: postId }).value()
```

**cryptr**
>加密解密   (这个加密注意一个情况,如果库文件存在的话会报错,只有初始化一个新的才可以)

```js
const Cryptr = require("./cryptr"),
const cryptr = new Cryptr('my secret key')

const db = low('db.json', {
  format: {
    deserialize: (str) => {
      const decrypted = cryptr.decrypt(str)
      const obj = JSON.parse(decrypted)
      return obj
    },
    serialize: (obj) => {
      const str = JSON.stringify(obj)
      const encrypted = cryptr.encrypt(str)
      return encrypted
    }
  }
})

db.defaults({ posts: [], user: {} })
  .write()
```

##使用问题总结

**set和unset的使用**
```js
//也可以操作,二维的数组,不过删除时最好不要用 unset ,会造成多一个`逗号`
//一般情况下造作一维比较好
db.unset('user.age').write()

db.set('posts[4].title', '啦啦啦啦')
  .write()
```

**加密解密**
> 这个问题上面已经说了,就是必须生成一个新文件,要不会报错json格式不正确


**使用lodash-id**
```js
//这个是正确的使用方式  不过这个插入真心意义不大和push 还不如push打字少
db.get('posts').insert({id:333, body: 'New post'}).write()

//----------------------------------------------------------------------

//只有write()的时候才真正写入, 如果没有write()只是返回不会修改文件
let dd = db.get('posts').replaceById(3,{body: '心心心心'}).write()

//----------------------------------------------------------------------

//这是原本更新和lodash-id比是稍显复杂写.而且这个和lodash-id都有个问题是,不能跟新所有只能跟新第一个
//因为这时我的库里有好几个 id:3 的数据
db.get('posts').find({id:3}).assign({body:'哈哈哈哈'}).write()  

//----------------------------------------------------------------------

//配合浏览器的时候 lodash-id 最有用
app.get('/posts/:id',(req,res)=>{
    console.log(typeof(req.params.id))
    //别说这个还是有点用的 如果用原版的找就找不到 可能是类型的关系
    //const post = db.get('posts').getById(req.params.id).value()

    const post = db.get('posts')
        .find({ id: parseInt(req.params.id) })  //就是类型问题
        .value()

    console.log(post)
    res.send(post)
})
```

**使用前一点要初始化数据库文件**
>曾经遇到过一次,因为清空数据.里面却保存了字符,结果再插入数据时报错
还有一次清空了数据.里面没有了表.插入数据时报错
而且不能初始化两次哦 否则看上去正常插入数据时也会报错

##官方例子
clis.js 用命行令使用方式
```js
"use strict"

//import low from 'lowdb'
const low = require('lowdb')
const db = low('db.json')

db.defaults({posts:[]})
    .write()

const result = db.get('posts')
    .push({name: process.argv[2]})
    .write()

console.log(result)

```

server.js 这里美精简 整个测试过程全在里面了 挑着用
```js
const  express = require('express')
const _ = require('lodash')
_.mixin(require('lodash-id'))
const low = require('lowdb')
const fileAsync = require('lowdb/lib/storages/file-async')

const app = express()

const db = low('db/db.json',{
  storage: fileAsync
})

//使用这句 才能使用 lodash-id
db._.mixin(_)

db.defaults({posts:[]})
    .write()

// var radnum = Math.floor(Math.random()*10)
// console.log(radnum)

// db.get('posts')
//   .push({ id: radnum, body: 'lowdb is awesome'})
//   .write()

// //这是更新
// db.get('posts').find({id:3}).assign({body:'哈哈哈哈'}).write()  

// //好像只能改变一维数组,要不就只能清空
// // db.set('user.name', 'typicode')
// //   .write()

// let v = db.get('posts').value()
// console.log(v)

//这个是正确的使用方式  不过这个插入真心意义不大和push 还不如push打字少
//db.get('posts').insert({id:333, body: 'New post333333333'}).write()

//这个和find区别也不大
let aa = db.get('posts').getById(3).value()
//console.log(aa)

//这还有点用比上面那个少 不过也只能改写一个 这个数据库里有两个3其实
//还有就是只有write()的时候才真正写入,也许这是上面异步的关系
let dd = db.get('posts').replaceById(3,{body: '心心心心'}).write()
console.log(dd)

console.log('---------------------')

let pp = db.get('posts').value()
console.log(pp)

app.get('/posts/:id',(req,res)=>{
    console.log(typeof(req.params.id))
    //别说这个还是有点用的 如果用原版的找就找不到 可能是类型的关系
    //const post = db.get('posts').getById(req.params.id).value()

    const post = db.get('posts')
        .find({ id: parseInt(req.params.id) })  //就是类型问题
        .value()

    console.log(post)
    res.send(post)
})

app.get('/posts',(req,res)=>{
    //low('posts').insert({ title: 'foo',id:Date.now() }).then(post=>{res.send(post)})
//     db.set('posts', {body:'呵呵',id:Date.now()})
//   .write()
//     console.log(db.posts)
     db.get('posts').insert({id:666, body: '双击屏幕666666666666'}).write().then(post=>res.send(post))
})

// app.get('/posts',(req,res)=>{
//     db.get('posts')
//         .push(req.body)
//         .last()
//         .assign({id:Date.now()})
//         .write()
//         .then(post=>res.send(post))
// })

//这应该是异步写法
// app.post('/posts', async (req, res) => {
//   const post = await db.get('posts')
//     .push(req.body)
//     .last()
//     .assign({ id: Date.now() })
//     .write()

//   res.send(post)
// })

db.defaults({posts:[]})
    .write()
    .then(()=>{
        app.listen(8080,()=>console.log('Server is listening'))
    })

//app.listen(8080,()=>console.log('Server is listening'))    
```
memory.js 内存模式这样应该会速度快很多吧
```js
const fs = require('fs')
const db = low()

db.defaults({ posts: [] })
  .write()

db.get('posts')
  .push({ title: 'lowdb' })
  .write()

// Manual writing
fs.writeFileSync('db.json', JSON.stringify(db.getState()))
```

##参考文档
[LowDB静态JSON文件数据库介绍](http://www.jianshu.com/p/11d04a4c22af)
