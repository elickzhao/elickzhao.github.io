---
title: webpack-dev-server 的 Node.js Api方式和CLI 方式 两种用法
tags:
  - node.js
  - 前端
  - webpack
abbrlink: 64787
date: 2017-06-22 22:40:27
categories:
---

CLI 方式会比Node.js Api方式简单很多.
Node.js Api方式 如果配置热启动 还需要配置 webpack.HotModuleReplacementPlugin 而且有时还得安装 webpack-hot-middleware 额外的插件

还有需要特别注意的地方是 **publicPath必填**
使用Node.js Api方式 结果还是不能自动更新.因为这个方法实在问题太多,最后还是使用了命令行

还有一点 入口配置一定要这么写 `entry: ["webpack-dev-server/client?http://localhost:8080"],`
```js
    //就到这里吧 热更新也做了 可惜就是不自动刷新浏览器
    //[WDS] App hot update... (执行到这里就不走了)
    //dev-server.js:45 [HMR] Checking for updates on the server... (就差这一句 不知道为啥)
    //这个写法真心没有命令行简单啊
    const server = new WebpackDevServer(
      compiler,
      {
        contentBase: path.join(__dirname, '../'),
        quiet: true,
        publicPath: '/dist/',   //publicPath必填 否则就不好使
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

**直接看这里吧**
[http://www.cnblogs.com/hhhyaaon/p/5664002.html](http://www.cnblogs.com/hhhyaaon/p/5664002.html)