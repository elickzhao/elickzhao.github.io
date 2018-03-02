---
title: webpack 零基础教程
date: 2016-10-11 21:43:17
tags: [js,前端,webpack]
categories: 前端技术
---


>这套教材非常好 非常适合我.
[Webpack傻瓜式指南（一）](https://zhuanlan.zhihu.com/p/20367175)
[Webpack傻瓜指南（二）开发和部署技巧](https://zhuanlan.zhihu.com/p/20397902)
[Webpack傻瓜指南（三）和React配合开发](https://zhuanlan.zhihu.com/p/20522487)

不过有个小问题: 里面的方法不能加括号`app.appendChild(sub());` 这个应当这么写 `app.appendChild(sub);` 这个错误原因可能是他手误了 

```js
//之前
//只需要 sub
//这个是CommonJS风格代码
function generateText(){
	var element = document.createElement('h2');
	element.innerHTML = "Hello hddddaa world";
	return element;
}
module.exports = generateText();

--------------------------------------------------------------------
//后面
// 所以需要sub()
//这个是ES6风格代码
export default function() {
  var element = document.createElement('h2');
  element.innerHTML = "Hello h2 world hahaha";
  return element;
}

```

<!--more-->

-----------------

这个教程又少写了一步:
在解决jquery老插件无法引入jquery的方法时
```js
//先得引用才行  教程里没有提这个
var webpack = require('webpack');
...

	plugins: [
		new HtmlWebpackPlugin({
			title: 'Hello world app'
		}),
		//new webpack.HotModuleReplacementPlugin(), //这是hot的一个插件
		//webpack提供一个插件 把一个全局变量插入到所有的代码中
		//这个就是插入jquery到所有文件中,应对jquery老的插件不支持引用
		new webpack.ProvidePlugin({
			$: "jquery",
			jQuery: "jquery",
			"window.jQuery": "jquery"
		})	
	],
```


> 配置hot时搜索的文档 `顺便说下hot 现在的感觉是同步组件修改 比如vue 不用这个插件 vue组件修改 浏览器不自动更新 这是我现在这么理解的`
[说说Webpack](http://my.oschina.net/bosscheng/blog/514638)
[七周七种前端框架二: React 之 webpack 简介](http://blog.csdn.net/lihongxun945/article/details/49942749)

>其他
[WebPack常用功能介绍](http://www.open-open.com/lib/view/open1450681593198.html) --> 这也是个很详细的文章
[webpack分离css单独打包](http://www.gowhich.com/blog/698)
[一小时包教会 —— webpack 入门指南](http://www.w2bc.com/Article/50764)
[Webpack 中文指南](http://webpackdoc.com/index.html)  -->手册包涵所有方面 但不是特别详细
[在node中使用babel6的一些简单分享](http://cnodejs.org/topic/56460e0d89b4b49902e7fbd3)  -->因为使用 `babel` 和 `babel-preset-es2015` 一直报错 `import` 所以查了这个