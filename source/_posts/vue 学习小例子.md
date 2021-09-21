---
title: vue 学习小例子
tags:
  - vue
  - 前端
  - js
categories: 前端技术
abbrlink: 22029
date: 2016-07-25 12:04:12
---

## 一些总结吧

- **父组件和子组件之间通信用广播:** vue可以用多种方法实现组件间沟通,比如用一个属性,
不过这必须用上同步 `:test.sync` 要不然子组件的状态父组件不知道.
但是这有个问题就是不利于解耦,捆绑太紧密了不能复用了,所以还是用广播.
不过还是有个小小的疑问,如果两个同名事件怎么办呢?? 按文档里说是第一个,那可能是父组件加载子组件的顺序吧
我想可能是这样的,如果存在同名的话,那就得手动调整顺序,我感觉最好不要同名.

- **v-bind:fields="columns" 绑定解释:** 有时用简写就比较懵,简单记忆下: 如果原有属性就是绑定属性到vue属性.如果不是自带属性,那就是组件自定义的.比如 `:fields="columns" ` 这里就是一个. fields 子组件 : columns 父组件. 反正等号后面都是当前组件的.可以看下面例子.

<!--more-->

```html
<!-- 绑定 attribute -->
<img v-bind:src="imageSrc">

<!-- 缩写 -->
<img :src="imageSrc">

<!-- 绑定 class -->
<div :class="{ red: isRed }"></div>
<div :class="[classA, classB]"></div>
<div :class="[classA, { classB: isB, classC: isC }]"></div>

<!-- 绑定 style -->
<div :style="{ fontSize: size + 'px' }"></div>
<div :style="[styleObjectA, styleObjectB]"></div>

<!-- 绑定到一个对象 --> <!-- 这个怎么用不太了解 --> 
<div v-bind="{ id: someProp, 'other-attr': otherProp }"></div>

<!-- prop 绑定，"prop" 必须在 my-component 组件内声明 -->
<my-component :prop="someThing"></my-component>

<!-- 双向 prop 绑定 -->
<my-component :prop.sync="someThing"></my-component>

<!-- 单次 prop 绑定 -->
<my-component :prop.once="someThing"></my-component>
```

- **v-on:create-item="createItem" 绑定事件监听器:** 绑定默认动作,如果不是默认动作,就是自定义事件.
` @create-item="createItem"` 这样的写法是把动作绑定到一个方法里.
如果没有绑定,就需要把 `create-item`写在`events` 属性里了
详细看下面例子代码

```html
<!-- 方法处理器 -->
<button v-on:click="doThis"></button>

<!-- 内联语句 -->
<button v-on:click="doThat('hello', $event)"></button>

<!-- 缩写 -->
<button @click="doThis"></button>

<!-- 停止冒泡 -->
<button @click.stop="doThis"></button>

<!-- 阻止默认行为 -->
<button @click.prevent="doThis"></button>

<!-- 阻止默认行为，没有表达式 -->
<form @submit.prevent></form>

<!-- 串联修饰符 -->
<button @click.stop.prevent="doThis"></button>

<!-- 键修饰符，键别名 -->
<input @keyup.enter="onEnter">

<!-- 键修饰符，键代码 -->
<input @keyup.13="onEnter">

<!--在子组件上监听自定义事件（当子组件触发 “my-event” 时将调用事件处理器）：-->
<my-component @my-event="handleThis"></my-component>

<!-- 内联语句 -->
<my-component @my-event="handleThis(123, $arguments)"></my-component>

```


```js
new Vue({
    el: '#app',
    store: store,
    template: '<App/>', //原来如此 这句相当于在 index.html里 #app容器里 添加了 组件<App><App/> 就是下面那个
    components: {App}
})
```

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>notepad</title>
    <meta name="viewport" content="width=device-width, initial-scale=1, user-scalable=no">
    <link href="favicon.ico" rel="SHORTCUT ICON">
</head>
<body>
<div id="app">
  <!-- <App></App>  这个和main.js里的template作用是一样的 如果在new vue()的时候写 这样更简洁些 不过不利于阅读而已 -->
</div>
<!-- built files will be auto injected -->
</body>
</html>
```


- **this.dataList.$set($index, { ... } 如何给数组修改数据:**`this.dataList[$index] = { ... } ` 这么写可以 不过不能刷新页面.
所以要修改数组数据 就得这么写 `this.dataList.$set(index, { ... }`
详细看下面例子详细代码

- **this.item = this.dataList[index]:** 这个真好不说,还不是十分明白.但是下面的例子确实是这样的.
如果赋值了 他们好像就是绑定了 因为在子组件修改时 父组件数据也同步变化了.根本不需要我再去写什么去修改这个dataList数组了.
详细看下面例子代码吧
    
**例子:**

<iframe width="80%" height="500" src="//jsfiddle.net/elick/s03Lk51x/2/embedded/result,js,html,css" frameborder="0"></iframe>
