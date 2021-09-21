---
title: 用 html5+js+css vue 开发个桌面应用 -Electron
tags:
  - js
  - 前端
  - vue
  - node.js
  - 小百科
  - Electron
abbrlink: 18271
date: 2017-06-24 21:01:43
categories:
---

## 简单说明
想用js开发桌面现在有两个框架可以用 **Electron** 另外一个是 **nwjs**.
但感觉还是用 **Electron**比较好,因为现在一些流行的软件是那这个弄的,比如说我现在用的 **Atom** ,**VSCode** 这两个软件都非常好用,而且漂亮.
另外还有个原因是,他有中文文档.这样实在是方便太多了


## 编写

这里主要编写个 **Electron** 启动配置文件.是 `package.json` 里的 `main` 字段的文件.
**注意**：如果 main 字段没有在 package.json 声明，Electron会优先加载 index.js。
剩下的就是编写自己的程序了就行了.

贴两个配置文件样本.
一 **这是官方的例子**
```js
const { app, BrowserWindow } = require('electron')
const path = require('path');
const url = require('url')

// 保持一个对于 window 对象的全局引用，如果你不这样做，
// 当 JavaScript 对象被垃圾回收， window 会被自动地关闭
let win

function createWindow() {
    // 创建浏览器窗口。
    win = new BrowserWindow({ width: 800, height: 600 })

    // 加载应用的 index.html。
    win.loadURL(url.format({
        pathname: path.join(__dirname, 'dist/index_prod.html'),
        protocol: 'file:',
        slashes: true
    }))
    //win.loadURL('http://localhost:8080')

    // 打开开发者工具。
    win.webContents.openDevTools();

    // 当 window 被关闭，这个事件会被触发。
    win.on('closed', () => {
        // 取消引用 window 对象，如果你的应用支持多窗口的话，
        // 通常会把多个 window 对象存放在一个数组里面，
        // 与此同时，你应该删除相应的元素。
        win = null
    })
}

// Electron 会在初始化后并准备
// 创建浏览器窗口时，调用这个函数。
// 部分 API 在 ready 事件触发后才能使用。
app.on('ready', createWindow)

// 当全部窗口关闭时退出。
app.on('window-all-closed', () => {
    // 在 macOS 上，除非用户用 Cmd + Q 确定地退出，
    // 否则绝大部分应用及其菜单栏会保持激活。
    if (process.platform !== 'darwin') {
        app.quit()
    }
})

app.on('activate', () => {
    // 在这文件，你可以续写应用剩下主进程代码。
    // 也可以拆分成几个文件，然后用 require 导入。
    if (win === null) {
        createWindow()
    }
})

// 在这文件，你可以续写应用剩下主进程代码。
// 也可以拆分成几个文件，然后用 require 导入。
```

<!--more-->

二 **这个项目[electron-anyproxy](https://github.com/fwon/electron-anyproxy/blob/master/main.js)的例子**
>贴这个例子是因为这里有个程序启动页面,这个不错
```js
const electron = require('electron');
const menuTemplate = require('./menu.js');
const ipcMain = electron.ipcMain;
const app = electron.app;
const Menu = electron.Menu;
const BrowserWindow = electron.BrowserWindow;
//启动页面参数
let loadingParams = {
    width: 580,
    height: 200,
    frame: false,
    show: false
};
//主页面参数
let mainParams = {
    width: 1300,
    height: 780,
    icon: __dirname + '/icon.png',
    // titleBarStyle: 'hidden-inset',
    backgroundColor: '#fff',
    show: false
};

let mainWindow;

function createWindow() {
    mainWindow = new BrowserWindow(mainParams);
    mainWindow.setTitle(require('./package.json').name);

    //setting in .vscode/launch.json
    //测试时可以这么写 但是必须启动静态服务器才行
    if (process.env.NODE_ENV === 'development') {
        console.log('develop');
        mainWindow.loadURL('http://localhost:4000');
        mainWindow.webContents.openDevTools();
    } else {
        //完成后打包时 必须加载生成的静态页才行.
        //这个写法比较好 比上面那个简单些
        mainWindow.loadURL(`file://${__dirname}/client/index.html`);
    }

    //这个监听页面加载完成显示
    mainWindow.webContents.on('did-finish-load', () => {
        mainWindow.show();
        //主页面加载完毕显示,把启动页关闭.
        if (loadingScreen) {
            loadingScreen.close();
        }
    });

    //测试工具 这是我添加的
    mainWindow.webContents.openDevTools();

    mainWindow.on('closed', () => {
        mainWindow = null;
    });
}

//创建启动页面
function createLoadingScreen() {
    //这里加了父页面的参数不知道是何用
    loadingScreen = new BrowserWindow(Object.assign(loadingParams, { parent: mainWindow }));

    if (process.env.NODE_ENV === 'development') {
        loadingScreen.loadURL('http://localhost:4000/loading.html');
    } else {
        loadingScreen.loadURL(`file://${__dirname}/client/loading.html`);
    }

    loadingScreen.on('closed', () => loadingScreen = null);
    //这个感觉是没必要的
    loadingScreen.webContents.on('did-finish-load', () => {
        loadingScreen.show();
    });
}

//这是个菜单模版
function createMenu() {
    const menu = Menu.buildFromTemplate(menuTemplate);
    Menu.setApplicationMenu(menu);
}

app.on('ready', () => {
    createLoadingScreen();
    createWindow();
    createMenu();
});

app.on('window-all-closed', () => {
    if (process.platform !== 'darwin') {
        app.quit();
    }
});

app.on('activate', () => {
    if (mainWindow === null) {
        createWindow();
    }
});

```


## 测试
测试话就用 **webpace** 打开服务器测试就好.然后 **electron** 打开页面.
但配置一定要这么写.这样你修改的时候浏览器和程序都会自动更新了,能即时发现问题.
```js
loadingScreen.loadURL('http://localhost:4000/loading.html');
```



## 打包
打包需要安装另外一个插件
[electron-packager](https://github.com/electron-userland/electron-packager)

这是简单的配置说明
>* location of project：项目所在路径
* name of project：打包的项目名字
* platform：确定了你要构建哪个平台的应用（Windows、Mac 还是 Linux）
* architecture：决定了使用 x86 还是 x64 还是两个架构都用
* electron version：electron 的版本
* optional options：可选选项
* ignore : 忽略打包目录  (但我测试没什么效果啊)
* overwrite : 好像是打包后会把上一个删除

这个插件可以全局安装,这样就可以使用全局命令 `electron-packager`.
当然也可以安装到项目里.然后放到 `package.json`里,使用 `npm`命令启动
```js
  "scripts": {
    "start": "electron .",
    "pack": "electron-packager . Anyproxy --out=pack --overwrite --ignore=client/node_modules --icon=icon.icns"
  },
```


打包会下载 **electron** 不同操作系统框架,但是这个包下载比较麻烦.还有另外一个打包方式
这就是另一个方式 其实就是指定了一个镜像
`ELECTRON_MIRROR=https://npm.taobao.org/mirrors/electron/ electron-packager`
但这里还有个问题是, 同时启动了 `webpack` 不知道是干什么用
这段出自 [TooNote](https://github.com/TooBug/TooNote/blob/master/src/package.json) 这个项目
```json
{
  "name": "TooNote",
  "version": "0.3.0",
  "description": "Markdown based note app.",
  "main": "electron.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "webpack -w",
    "prod": "NODE_ENV=production webpack",
    "prod-win": "set NODE_ENV=production&& webpack",
    "run": "electron .",
    "start": "npm-run-all -p -r dev run",
    "cloud": "CLOUD=1 npm-run-all -p -r dev run",
    "build:osx": "npm run prod&& ELECTRON_MIRROR=https://npm.taobao.org/mirrors/electron/ electron-packager . --overwrite --arch=all --icon=../assets/logo.icns --out=../dist --ignore=\"^/(?:api|component|models|modules|vuex)\" --ignore=\"/windows\" --ignore=\"\\.map$\" --ignore=\"node_modules/(?:d[$/]|babel-core|es5\\-ext|vue/src|vue/types|remarkable/dist|[\\w\\-]+/benchmark|[\\w\\-]+/test|[\\w\\-]+/docs|[\\w\\-]+/demo|brace/mode/[^m])\"",
    "build:win": "npm run prod-win&& set ELECTRON_MIRROR=https://npm.taobao.org/mirrors/electron/&& electron-packager . --overwrite --arch=all --icon=../assets/logo.ico --out=../dist --win32metadata.CompanyName=\"TooNote\" --win32metadata.ProductName=\"TooNote\" --win32metadata.FileDescription=\"TooNote\" --ignore=\"^/(?:api|component|models|modules|vuex)\" --ignore=\"/osx\" --ignore=\"\\.map$\" --ignore=\"node_modules/(?:d[$/]|babel-core|es5\\-ext|vue/src|vue/types|remarkable/dist|[\\w\\-]+/benchmark|[\\w\\-]+/test|[\\w\\-]+/docs|[\\w\\-]+/demo|brace/mode/[^m])\""
  },
```

**打包加密**
此时打包完毕,但是源文件目录会放在`resources`,这总让人感觉不太舒服.这时就需要 `asar`了.

安装
```js
npm install --save-dev asar
```
使用:同上面一样放在package.json 当作一个命令.当然也可以全局安装.
```
"build:asar": "asar pack . app.asar"

---
npm run build:asar
```

发现打包完毕,把 `app.asar` 复制到 `resources` 里,然后把源文件目录 `app` 删除就好了
启动看看吧 应该成功了


**注意**:
下面这句可能是谣传啊,我测试后发现大小好像没有明显改变. 我又试了几次 完全没看出效果来
> - 打包时建议用yarn安装npm包，因为npm install会在node_modules中安装隐藏目录，导致electron-packager打包的时候无法将electron等大文件删除，打包出来的软件包会很大。
> - 安装 electron 包过慢（国内情况）的解决方法：
临时方式： DEBUG=* ELECTRON_MIRROR="https://npm.taobao.org/mirrors/electron/" npm install electron
加入DEBUG=*是为了查看调试信息，确认下载源是否替换成功。
我再windows下使用这个命令不好使啊



## 扩展包
[Electron社区 这里有很多扩展包](https://electron.atom.io/community/)
|包名|说明|
|--|--|
|[electron-packager](https://github.com/electron-userland/electron-packager)| 打包成绿色包 直接能用
|[electron-builder](https://github.com/electron-userland/electron-builder)| 打包成安装包
|[vue-electron](https://github.com/SimulatedGREG/vue-electron) |好像是让vue程序 可以用electron APIs
|[electron-vue](https://github.com/SimulatedGREG/electron-vue) |这是个模版项目 . `vue-init simulatedgreg/electron-vue my-project` 这样使用.
||linux和windows命令不同要注意.这里的是windows的,linux是 vue init|
|[asar](https://github.com/electron/asar)|可以把程序打包成asar,这样看起来更简洁,而且容易出现胡乱改动,造成程序不可用|


electron-vue 命令解释
`pack`: 这是指原来的build 打包静态页 打包后放在 dist/electron里
`pack:renderer`按道理这个应该放在dist/web下的但却没有
`build` 结果这个是打包成exe文件 还有一点千万不要用 electron-builder 真的很差 不能选路径 界面很丑


## 待解决
~~只是把vue打包好的静态文件加载到了 Electron 这一步倒是很简单
我现在想做到的是,`vue` 不用 `build`,直接用 `Electron` 打包成程序,但好像是不行啊.
打完包后源文件还在,而且不能删除,这个好像有点不对吧,应可以删除的吧,以后可以看看~~

---
第一个问题解决方法经过查看是,把两部写成一个命令就好了
第二个问题使用asar打包成asar包,就可以把源文件给删除了.其实也是只防君子不防小人.


## 参考文档
[用HTML5+JS开发跨平台的桌面应用](http://www.cnblogs.com/terrylin/p/4938693.html)
[用Electron开发桌面应用](http://get.ftqq.com/7870.get)
[Electron Github仓库](https://github.com/electron/electron)
[Electron 中文文档](https://github.com/electron/electron/tree/master/docs-translations/zh-CN)
[nwjs官方网站](http://nwjs.io/)
[这里有两个例子,其中一个是vue的](https://www.zhihu.com/question/36644309)
[【Electron】Electron开发入门（五）：项目打包](http://blog.csdn.net/arvin0/article/details/52690023)
[XCel 项目总结 - Electron 与 Vue 的性能优化](https://aotu.io/notes/2016/11/15/xcel/)
[TooNotez 一个实例项目](https://github.com/TooBug/TooNote)
[使用 Electron 开发一款理财计算器](https://blog.callmewhy.com/2016/07/01/make-calculator-with-electron/)
[从零开始使用Electron + jQuery开发桌面应用 （二） 打包应用](https://segmentfault.com/a/1190000004863646)
