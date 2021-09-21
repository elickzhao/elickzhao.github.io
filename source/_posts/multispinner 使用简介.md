---
title: multispinner 使用简介
tags:
  - node.js
  - 小百科
abbrlink: 11700
date: 2017-07-10 20:55:01
categories:
---


![](https://raw.githubusercontent.com/codekirei/node-multispinner/master/extras/multispinner.gif)

multispinner 是在命令行执行时等待的样式,并且能根据结果返回不同的提示信息.

![](https://raw.githubusercontent.com/codekirei/node-multispinner/master/extras/demo.gif)

这个node插件使用起来非常简单 在[multispinner Github](https://github.com/codekirei/node-multispinner) 里有例子 不太懂可以看一下.

<!--more--!>

**普通方法使用**
```js
'use strict'

// setup
const Multispinner = require('Multispinner')
const spinners = ['foo', 'bar', 'baz', 'qux']   //这是设定同时进行几个任务执行

// instantiate and start spinners
const m = new Multispinner(spinners)

// finish spinners in staggered timeouts
setTimeout(() => m.success('foo'), 1000)    //这里是任务根据结果返回 信息
setTimeout(() => m.error('bar'), 1500)
setTimeout(() => m.success('baz'), 2000)
setTimeout(() => m.error('qux'), 2500)

// do something on success/error events
// 这里是定义返回信息输出什么内容
m.on('success', () => {
  // does not fire in this example because m.error is called
  console.log('done without errors!')
}).on('err', (e) => {
  console.log(`${e} spinner finished with an error`)
})

```
<!--more-->

**自定义样式**
```js
'use strict'

// modules
const Multispinner = require('Multispinner')
const figures = require('figures')  //这一个符号小插件

// constants
const spinners = ['task A', 'task B', 'task C']
const opts = {
  'interval': 120,
  'preText': 'Completing',  // (*) Completing task A
  'postText': 'process',    //这是后缀
  'frames': [
    '[          ]',
    '[>         ]',
    '[)>        ]',
    '[￣)>      ]',
    '[3￣)>     ]',
    '[￣3￣)>   ]',
    '[(￣3￣)>  ]',
    '[<(￣3￣)> ]',
    '[<(≧ □ ≦)>]',
    '[ <(≧ □ ≦)]',
    '[  <(≧ □ ≦]',
    '[   <(≧ □ ]',
    '[    <(≧ □]',
    '[     <(≧ ]',
    '[      <(≧]',
    '[       <(]',
    '[        <]',
    '[         ]',
  ],
  'symbol': {
    'success': ' '.repeat(7) + figures.tick
  }
}

const opts1 = {
  'interval': 150,  //数值越大 速度越慢
  'preText': 'Completing',  //这个是输出信息前,加的前缀内容
  'frames': [
    '[         ]',
    '[>        ]',
    '[)>       ]',
    '[￣)>     ]',
    '[3￣)>    ]',
    '[￣3￣)>  ]',
    '[(￣3￣)> ]',
    '[<(￣3￣)>]',
    '[ <(￣3￣)]',
    '[  <(￣3￣]',
    '[   <(￣3 ]',
    '[    <(￣3]',
    '[     <(￣]',
    '[      <( ]',
    '[       <(]',
    '[        <]',
    '[         ]',
    '[         ]',
  ],
  'symbol': {//repeat()是重复字符串几次 前面是空格 也就是重复空格几次
    'success': ' '.repeat(7) + figures.circleFilled
  }
}

// initialize
const m = new Multispinner(spinners, opts1)

// staggered completion
const t = 2500
let i = 0
function loop() {
  setTimeout(() => {
    m.success(spinners[i])
    i++
    if (i < spinners.length) loop()
  }, t)
}
loop()

```


**实际应用**
```js

  //实际应用中要配合 Promise 使用

  const tasks = ['renderer','app']
  const m = new Multispinner(tasks, {
    preText: 'building',
    postText: 'process'
  })

    m.on('success', () => {
    process.stdout.write('\x1B[2J\x1B[0f')
    //console.log(`\n\n ${results} `)
    console.log(` ${okayLog}take it away ${chalk.yellow('`web-builder`')}\n`)
    process.exit()
  })

  app().then(() => {
    m.success('app')
    console.log(` ${okayLog}take it away ${chalk.yellow('`app-builder`')}\n`)
    process.exit()
  }).catch((err) => {
    m.error('app')
    console.log(`\n  ${errorLog}failed to build app process`)
    console.error(`\n${err}\n`)
    process.exit(1)
  })

  function app() {
  return new Promise((resolve, reject) => {
    exec('electron-packager . HelloWorld windows --out ./OutApp --version 1.6.11 --overwrite --ignore=client/node_modules', (error, stdout, stderr) => {
      if (error) {
        reject(stderr)
      } else {
        resolve(stdout)
      }
    })

  })
}

```
