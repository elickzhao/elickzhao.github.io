---
title: Sea.js 模块开发插件
tags:
  - js
  - 前端
  - 小百科
categories: 前端技术
abbrlink: 34544
date: 2016-06-02 22:26:58
---

## 前言

  现在大部分前端开发,都用npm或者bower.因为这样不仅升级组件方便,而且兼容性也能得到保障.但是对于简单开发来说,过于庞大的组件也是比较麻烦的.所以这个Sea.js还是有一定存在价值的.

  我也是在KodExplorer里发现这个插件的.没有太多使用,所以这里就点一下,以便以后记住.如果需要用就看官方吧,里面文档很详细的.

<!--more-->

## 我的一些经验

定义模块的时候,两种方法对外提供功能.但是不能在同一文件中,定义两个.一种是提供函数,另一种是提供类的方法.两种使用方法也不同,具体看下面例子

<!--more-->

模块js
```js
define(["./languages/en", 
                "./plugins/link-dialog/link-dialog",
                "./plugins/reference-link-dialog/reference-link-dialog",
                "./plugins/image-dialog/image-dialog",
                "./plugins/code-block-dialog/code-block-dialog",
                "./plugins/table-dialog/table-dialog",
                "./plugins/emoji-dialog/emoji-dialog",
                "./plugins/goto-line-dialog/goto-line-dialog",
                "./plugins/help-dialog/help-dialog",
                "./plugins/html-entities-dialog/html-entities-dialog", 
                "./plugins/preformatted-text-dialog/preformatted-text-dialog"
            ],
			function(require, exports, module){
	//var $ = require("jquery");
	
	//这里很怪 define([加载模块组]) 这里说是预加载,可是没有下面 require()是用不了的
	//但是下面require() 并不需要上面定义预加载 也可以用 那么这数组还有什么意思
	//但是要改的那里 也是这么写的 好怪
	
	require("./plugins/link-dialog/link-dialog");
	require("./plugins/reference-link-dialog/reference-link-dialog");
	require("./plugins/image-dialog/image-dialog");
	require("./plugins/code-block-dialog/code-block-dialog");
	require("./plugins/table-dialog/table-dialog");
	require("./plugins/emoji-dialog/emoji-dialog");
	require("./plugins/goto-line-dialog/goto-line-dialog");
	require("./plugins/help-dialog/help-dialog");
	require("./plugins/html-entities-dialog/html-entities-dialog");
	require("./plugins/preformatted-text-dialog/preformatted-text-dialog");
	var editormd = require("./editormd");
	//console.log($, editormd);
	//console.log(require.resolve("./editormd"));
	var uri = (module.uri);
	//console.log(uri.substr(0,(uri.length-11))); 
	var libpath = uri.substr(0,(uri.length-11))+"lib/";
	
	//exports.foo = 'bar';
	exports.doSomething = function() {
	 	//alert('dosomething');
	 			        testEditor = editormd("test-editormd", {
		            width: "90%",
		            height: 640,
		            path : libpath,
		            markdown : 'ddddd',
		            //toolbar  : false,             //关闭工具栏
		            htmlDecode : true,            // 开启HTML标签解析，为了安全性，默认不开启
		            tex : true,                   // 开启科学公式TeX语言支持，默认关闭
		            //previewCodeHighlight : false,  // 关闭预览窗口的代码高亮，默认开启
		            flowChart : true,              // 疑似Sea.js与Raphael.js有冲突，必须先加载Raphael.js，Editor.md才能在Sea.js下正常进行；
		            sequenceDiagram : true,        // 同上
		            onload : function() {
		                console.log('onload', this);
		                //this.fullscreen();
		                //this.unwatch();
		                //this.watch().fullscreen();
		
		                //this.setMarkdown("#PHP");
		                //this.width("100%");
		                //this.height(480);
		                //this.resize("100%", 640);
		            }
		        });
	 	
	 };
	
	function Mdeditor () {
		
	}
	
	//这两个是冲突的 module.exports和exports只能出现一个
	//module.exports 返回的是个对象,exports返回的是属性和方法
	//module.exports = Mdeditor;
	
	Mdeditor.prototype.call = function () {
		alert('ddd');
	}
	
  
});
```

mainjs
```js
	var editormd =  require("editormd");
	editormd.doSomething();
	alert(editormd.foo);
	//当使用对象时 还得new一下才可以
//	var e = new editormd();
//		e.call();
```

----

在seajs.use()的时候加载的模块是自动引用的,不用想上面的定义模块时,require一下,直接就可用了.

```js
// // 并发加载模块 a 和模块 b，并在都加载完成时，执行指定回调
seajs.use(['./a', './b'], function(a, b) {
  a.init();
  b.init();
});


------------------------------------------------------------------

<script type="text/javascript">
            seajs.config({
                base  : "./",
                alias : {
                    jquery   : "js/jquery.min",
                    //editormd : "../../editormd"
                    editormd : "../editormd"
                }
            });
                
            //seajs.use("./js/seajs-main"); //使用main.js时 editormd 路径要改为 "../../editormd"
            
            var deps = [
                "jquery", 
                "editormd", 
                "../languages/en", 
                "../plugins/link-dialog/link-dialog",
                "../plugins/reference-link-dialog/reference-link-dialog",
                "../plugins/image-dialog/image-dialog",
                "../plugins/code-block-dialog/code-block-dialog",
                "../plugins/table-dialog/table-dialog",
                "../plugins/emoji-dialog/emoji-dialog",
                "../plugins/goto-line-dialog/goto-line-dialog",
                "../plugins/help-dialog/help-dialog",
                "../plugins/html-entities-dialog/html-entities-dialog", 
                "../plugins/preformatted-text-dialog/preformatted-text-dialog"
            ];
			
            seajs.use(deps, function($, editormd) {
                var testEditor;
                
                $.get("./test.md", function(md){
                    testEditor = editormd("test-editormd", {
                        width: "90%",
                        height: 640,
                        path : '../lib/',
                        markdown : md,
                        //toolbar  : false,             // 关闭工具栏
                        codeFold : true,
                        searchReplace : true,
                        saveHTMLToTextarea : true,      // 保存 HTML 到 Textarea
                        htmlDecode : "style,script,iframe|on*",            // 开启 HTML 标签解析，为了安全性，默认不开启
                        emoji : true,
                        taskList : true,
                        tocm            : true,          // Using [TOCM]
                        tex : true,                      // 开启科学公式 TeX 语言支持，默认关闭
                        //previewCodeHighlight : false,  // 关闭预览窗口的代码高亮，默认开启
                        flowChart : true,                // 疑似 Sea.js与 Raphael.js 有冲突，必须先加载 Raphael.js ，Editor.md 才能在 Sea.js 下正常进行；
                        sequenceDiagram : true,          // 同上
                        //dialogLockScreen : false,      // 设置弹出层对话框不锁屏，全局通用，默认为 true
                        //dialogShowMask : false,     // 设置弹出层对话框显示透明遮罩层，全局通用，默认为 true
                        //dialogDraggable : false,    // 设置弹出层对话框不可拖动，全局通用，默认为 true
                        //dialogMaskOpacity : 0.4,    // 设置透明遮罩层的透明度，全局通用，默认值为 0.1
                        //dialogMaskBgColor : "#000", // 设置透明遮罩层的背景颜色，全局通用，默认为 #fff
                        imageUpload : true,
                        imageFormats : ["jpg", "jpeg", "gif", "png", "bmp", "webp"],
                        imageUploadURL : "./php/upload.php",
                        onload : function() {
                            console.log('onload', this);
                            //this.fullscreen();
                            //this.unwatch();
                            //this.watch().fullscreen();

                            //this.setMarkdown("#PHP");
                            //this.width("100%");
                            //this.height(480);
                            //this.resize("100%", 640);
                        }
                    });
                });
              });
        </script>


```

  
## 项目地址

  [http://seajs.org/docs/#intro](http://seajs.org/docs/#intro)