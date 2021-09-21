---
title: vue-admin 启动服务器测试报错 require not define 解决
tags:
  - js
  - node.js
  - 小百科
  - vue
abbrlink: 61537
date: 2016-12-05 22:05:53
categories:
---

原因就是chart.js需要Chart.bundle.js支持. 不过 vue-admin 把chart.js设置成不解析依赖的了,所以就会报错.
这个配置就在 79 行.

webpace.base.conf.js
```js
var path = require('path')
var config = require('../config')
var utils = require('./utils')
var projectRoot = path.resolve(__dirname, '../')

module.exports = {
  entry: {
    app: './src/main.js'
  },
  output: {
    path: config.build.assetsRoot,
    publicPath: config.build.assetsPublicPath,
    filename: '[name].js'
  },
  resolve: {
    extensions: ['', '.js', '.vue'],
    fallback: [path.join(__dirname, '../node_modules')],
    alias: {
      'src': path.resolve(__dirname, '../src'),
      'assets': path.resolve(__dirname, '../src/assets'),
      'components': path.resolve(__dirname, '../src/components')
    }
  },
  resolveLoader: {
    fallback: [path.join(__dirname, '../node_modules')]
  },
  module: {
    preLoaders: [
      {
        test: /\.vue$/,
        loader: 'eslint',
        include: projectRoot,
        exclude: /node_modules/
      },
      {
        test: /\.js$/,
        loader: 'eslint',
        include: projectRoot,
        exclude: /node_modules/
      }
    ],
    loaders: [
      {
        test: /\.vue$/,
        loader: 'vue'
      },
      {
        test: /\.js$/,
        loader: 'babel',
        include: projectRoot,
        // /node_modules\/(?!vue-bulma-.*)/
        exclude: new RegExp(`node_modules\\${path.sep}\(\?\!vue-bulma-.*\)`)
      },
      {
        test: /\.json$/,
        loader: 'json'
      },
      {
        test: /\.html$/,
        loader: 'vue-html'
      },
      {
        test: /\.(png|jpe?g|gif|svg)(\?.*)?$/,
        loader: 'url',
        query: {
          limit: 10000,
          name: utils.assetsPath('img/[name].[hash:7].[ext]')
        }
      },
      {
        test: /\.(woff2?|eot|ttf|otf)(\?.*)?$/,
        loader: 'url',
        query: {
          limit: 10000,
          name: utils.assetsPath('fonts/[name].[hash:7].[ext]')
        }
      }
    ],
    //忽略已知文件解析
    noParse: [
      // /chart\.js/,   //我去这个真的好使了,看这个插件还是有依赖的,所以不能忽略
      /handsontable\.(full\.)?js/,
      /plotly\.js/
    ],
  },
  eslint: {
    formatter: require('eslint-friendly-formatter')
  },
  vue: {
    loaders: utils.cssLoaders()
  }
}

```

## 参考文档
[Uncaught ReferenceError : require is not defined - Chart.js](http://stackoverflow.com/questions/38176769/uncaught-referenceerror-require-is-not-defined-chart-js)
