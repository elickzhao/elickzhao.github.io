---
title: vue + iview后台实例问题总结  vue学习问题总结
tags:
  - vue
  - js
abbrlink: 5080
date: 2017-06-13 23:20:45
categories:
---


## vue组件模版编写规则
**main.vue** 在组件里 所有的样式必须放在一个 `<div></div>` 标签里 否则会出错.
 这个标签可以指定 默认为 `class=index` 这个看教程时已经知道,只是做的时候又给忘记了

---

## vue组件开始之前执行组件方法的方法.呵呵
1. **Vue.nextTick(function () {   })**
>在下次 DOM 更新循环结束之后执行延迟回调。在修改数据之后立即使用这个方法，获取更新后的 DOM。 因为有的插件需要操作DOM 但是DOM还没创建出来时 会出现问题  **这个也是如此无法用this** [我理解的关于Vue.nextTick()的正确使用](https://segmentfault.com/a/1190000008570874)

2.  **created:function(){}**
>这个也可以的 这是钩子程序 但是这个时期 DOM没有创建出来

3. **mounted: function (){}**
> 这个也是vue生命周期中一个钩子,这个就是加载完组件,生成了DOM的时候,所这个可以用,需要对DOM进行操作的程序可以放在这里执行

```js
// 记性真是不好上一次还知道怎么用,下一次就又忘了 现在放在这
export default {
    mounted: function () {
        particlesJS('particles-js', particles_config)
    },
    methods: {
    ...
    }
```


<!--more-->

4.**beforeRouteEnter(to, from, next) {}**
>这是vue-router的一个钩子,在进入路由之前调用一个函数,这个也是可行的,这个有时会有问题 如果路由没跳转,那么就不会执行了 **但是无法执行组件的方法,因为this是不存在的**
用另一个方法就可以使用组件内部的方法了,**而且更神器的是插件居然执行了**. `new Particleground.particle('#canvas', options);` 这个插件一直不执行的,只有触发他所在的方法`handleStart()`才会执行.
```js
    beforeRouteEnter(to, from, next) {
        next(vm => {
            // 通过 `vm` 访问组件实例
            vm.handleStart()
        });
    },   methods: {
        handleStart() {
            var options = {
                color: '#FFF', //['#2d8cf0', '#19be6b', '#ff9900', '#ed3f14'],
                num: 100,
                range: 300,
                linewidth: 1,
                maxSpeed: 1,
                maxR: 5,
            }
            //这个就更奇葩了.因为是类 不是函数 结果必须在方法里靠点击才能运行 其他的一概不执行 而且他运行也不像文档里写的必须引入particles.js 也够怪的
            new Particleground.particle('#canvas', options);
            console.log("cccccccccccccc");
        }
```

5.又找到个办法可以使用 `new Particleground.particle('#canvas', options);`就是这么些

```js
    var options = {
    color: '#FFF', //['#2d8cf0', '#19be6b', '#ff9900', '#ed3f14'],
    num: 100,
    proximity:100,  //点点连线距离,只有两点小于这个距离才会连线,值太大的话连线会很密的
    range: 1300,    //点点连线的范围 值越大范围越大 太小的话即使达到距离 也有些点不连线
    linewidth: 1,
    maxSpeed: 1,
    maxR: 5,
    lineShape:'cube',
}

//这个效果和上面的 beforeRouteEnter(to, from, next) {} 是一直的 而且我这次升级了 Particleground
//现在叫JParticles 参数更多了一些
//呵呵 又发现个区别啊  document.ready 就不可以 只能用 window.onload
window.onload = function(){
    new JParticles.particle('#canvas', options);
}
```

6.这三个的加载顺序很有趣, beforeRouteEnter -> Vue.nextTick -> created 看来created是创建组件的时候

7.~~放在export default {}的js代码也会被执行的,而且好像是页面渲染完成后执行的.~~ **这句当时不知道怎么写的,好像是错了.** 这里只能放,函数和属性,是没法使用js代码段的

---

## 老插件及不能用NPM安装的插件引用
用**webpack**创建的项目,引用这种插件很费事.而且不能用CND,也许能用只是我没找到方法.
看下面的文章,有好几个方法,不过都很麻烦.
不过我发现好像直接用 `import particlesJS from '../libs/particles.js';`
也是能找到正确位置的,但是否可用没测试 以后再说吧.

下面这个方法是我现在用的.
`var particlesJS = require('exports-loader?window.particlesJS!../libs/particles.js');`
记住一定用 **npm** 安装 **exports-loader**  这样就可以使用自己下载的插件了.
而且还解决了另外一个问题.以为**particlesJS**是个老插件,他没有现代的模块方式开发,所以用**npm**安装了,**import**了也是无法使用的.所以必须用**exports-loader**转换一下,这样**particlesJS**的方法就可用了.

参考文档
[如何在 vue 项目里正确地引用 jquery 和 jquery-ui的插件](https://segmentfault.com/a/1190000007020623)

---

## 插件的选择,有些插件不可以用.

这次就遇到了这个问题.
一 有可能是插件太老,引用后无法使用.会提示这个不是方法.
二 就是这个插件写的有问题,使用时是个类必须new 比如 `new Particleground.particle('#canvas', options);` 这个不是方法,所以无法自动执行,必须放在vue的方法里触发才可以.幸好如上面讲到的,
用vue-router里的钩子程序能触发 所以就一点用都没有了. 至于这个插件的写法以后研究下.


---
## webpace打包后单击静态文件显示空白内容

看到 `<router-view></router-view>` 加载内容为空.判断为这个插件出问题了.

最后发现,是 `vue-router` 的配置,`mode` 这个有三个选项 `"hash" | "history" | "abstract"`
必须选择 `hash`,否则的话必须用静态服务器访问才行,单击打开文件是不行的.
`history`的 url路径会美观不少,不过就是必须用静态服务器了

**注意:**
1. 在用本地服务器测试的时候. 用`nodejs`写的简易服务器,必须装载 `express`模块.
我这里并没有把`express`装到全局里,所以在其他目录是不好用的.但是项目目录里因为`webpace`使用了`express`,所以没有特别装也没问题.
2. 用 `phpstudy `充当服务器的时候,必须指定域名.如果不指定域名的话也是一片空白的.

```js
// 路由配置
const RouterConfig = {
    mode: 'hash',   //哦 的确如此 必须用hash才能打包成静态页
    routes: Routers
};
```

----
## Node.js Api方式使用

就是把命令行模式,转换写到程序里.不过这个方式写起来太复杂了
哦 热启动还有一点 如果不行这么写 `entry: ["webpack-dev-server/client?http://localhost:8080"],`

```js
   const compiler = webpack(rendererConfig)

       //就到这里吧 热更新也做了 可惜就是不自动刷新浏览器
    //[WDS] App hot update... (执行到这里就不走了)
    //dev-server.js:45 [HMR] Checking for updates on the server... (就差这一句 不知道为啥)
    //这个写法真心没有命令行简单啊
    const server = new WebpackDevServer(
      compiler,
      {
        contentBase: path.join(__dirname, '../'),
        quiet: true,
        publicPath: '/dist/',
        hot: true,
        compress: true,
        historyApiFallback: true,
        setup(app, ctx) {
          app.use(hotMiddleware)
          ctx.middleware.waitUntilValid(() => {
            resolve()
          })
        }
      }
    )
    server.listen(8080)
```

[webpack-dev-server 的 Node.js Api方式和CLI 方式 两种用法](http://elickzhao.github.io/2017/06/webpack-dev-server%20%E7%9A%84%20Node.js%20Api%E6%96%B9%E5%BC%8F%E5%92%8CCLI%20%E6%96%B9%E5%BC%8F%20%E4%B8%A4%E7%A7%8D%E7%94%A8%E6%B3%95/)

---
## webpack输出文件设置

这里 `path`已经设置`publicPath`不设置也可以,设置的话有可能出现引入错误
`filename` 也不用设置位置了.这样直接全部生成在 `dist` 目录下
如果`publicPath`设置了,css和js会保存在那里.但是html会在根目录

**注意**: ~~总是忘记啊 **json** 配置文件里是不能添加注释的 否则会报错~~ 是 **package.json** 注释会报错

```js
    output: {
        path: path.join(__dirname, './dist')
    },
    output: {
        publicPath: '',
        filename: '[name].[hash].js',
        chunkFilename: '[name].[hash].chunk.js'
    },
        new HtmlWebpackPlugin({
            filename: 'index_prod.html',
            template: './src/template/index.ejs',
            inject: false
        })    
```

---
## 在iview里build和dev模版

在iview里dev的时候用的是根目录的 **index.html** 因为dev的时候是css等静态文件是不加版本号
所以可以在**index.html**里写死 `<script type="text/javascript" src="/dist/main.js"></script>`

在build的时候,用的是 `src/template/index.ejs` 所以这里必须加这个 `<script type="text/javascript" src="<%= htmlWebpackPlugin.files.js[1] %>"></script>` 否则就无法添加带版本号的静态文件了

所以 在iview里 build 和 dev 是分开的. 他们的 `config`文件也不一样所以修改时要注意


---
## 这里需要修改啊
现在遇到的问题是我的 粒子插件的配置文件遇到了跨域请求问题. 而且他不能自动拷贝到`dist`目录下.
虽然报错写着这个,目录没有这个文件,可是拷过去依然没用

----
## 版本号插件输出信息问题
因为在做 **electron** 的时候,打包时需要用个效果插件.但是`process`其他信息都不会输出,唯独这个会输出个信息,应该是那里有问题,但是并影响使用,所以到现在也没仔细研究.就算 **webpack**  的显示进度 也会被他打断.

**这个以后有空解决下吧**

~~还有一点是在强调下,json不能有注释. 唉 总忘记~~ 这句是错的是 **package.json**不能有注释


```js
            test: /\.vue$/,
            loader: 'vue-loader',
            options: {
                loaders: {
                    css: ExtractTextPlugin.extract({
                        use: ['css-loader', 'autoprefixer-loader'], //加这个 autoprefixer-loader 就报错  有个输出 以后再考虑吧 反正现在可以用了 就是不是最完美样子
                        //use: ['css-loader'],
                        fallback: 'vue-style-loader'
                    })
                }
            }
        },
              {
            test: /\.css$/,
            use: ExtractTextPlugin.extract({    //这里去除这个插件就会找不到带版本号的插件了
                use: ['css-loader?minimize', 'autoprefixer-loader'],  // 这个也是 这个autoprefixer-loader 好像是版本选择
                //use: ['css-loader?minimize'],  
                fallback: 'style-loader'
            })
        },

```
---
## 简易服务器返回静态文件这么写

```js
app.get('/', function (req, res) {
 //res.send('Hello World')     //这里注意下因为生成的静态页不是index.html标准名称 所以不会显示
  res.sendFile( path.join(__dirname,  './dist/index_prod.html'))    //一定是绝对路径
})
```

----

## 使用 yarn 千万要注意
`yarn` 虽然速度比 `npm` 快很多,但总会出现莫名其妙的问题.我有个项目前两天运行正常,今天再打开一大堆报错.说各种文件找不到
用 `yarn` 重装一遍也不行,没办法用 `npm`重装 一切都好了

所以 用`yarn`千万要小心, 要不项目一开始就用,千万别中间再用

---

## webpack复制指定目录到dist生成目录
使用这个插件[copy-webpack-plugin](https://github.com/kevlened/copy-webpack-plugin)就ok了
用法很简单
```js
var CopyWebpackPlugin = require('copy-webpack-plugin');
var path = require('path');

    plugins: [
        new CopyWebpackPlugin([{    //还有就是这个参数必须是数组才行,哪怕只有一个
            from:'./src/assets',
            to:'assets' //因为上面已经有了输出路径,所以不需要写两次了
        }]),
        ......
        ]
```

----

## vue报错 template or render function not defined

**问题产生:** [独立构建-vs-运行时构建](http://cn.vuejs.org/v2/guide/installation.html#独立构建-vs-运行时构建) 这就是问题根源,webpack运行的不是完整vue,所以不包含解析`template`.这里需要在`webpack`设置里天上这句.        
```js
alias: {
            'vue': 'vue/dist/vue.esm.js'
        }
```
有的地方说是 `vue.common.js` 其实一样看下面官方说明
[不同构建版本的解释说明](https://vuefe.cn/v2/guide/installation.html#不同构建版本的解释说明)

**问题解决:**
但是我这个不是这么解决的,这还多亏了,vue-router里的提示

```js
    components:{
        //原本应该这么写  ff:require(['./aa.vue']) 就可以了 不知道这个 resolve 是哪里起的作用以后查查
        ff:(resolve) => require(['./aa.vue'], resolve)  //这么写组件就可以用了
    }

```

---

下面两个文章都不错,第一个讲解的非常深入,第二个是最开始的解决办法,现在好像不实用了,不过也可以了解下问题的起因.

[从一个奇怪的错误出发理解 Vue 基本概念](http://blog.csdn.net/csdn_yudong/article/details/59109902)
[vue2.0组件注册的问题](https://segmentfault.com/q/1010000007099099?_ea=1236628)
