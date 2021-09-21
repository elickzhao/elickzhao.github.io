---
title: js 操作文件 FileAPI
tags:
  - JavaScript
  - 前端
  - 小百科
categories: 前端技术
abbrlink: 28836
date: 2016-05-31 18:27:46
---
[TOC]

## 项目地址:
[https://github.com/mailru/FileAPI](https://github.com/mailru/FileAPI)

---

## 前言
    原本打算是操作本地文件,如打开文件,新建文件,保存文件.但好像js是无法坐到这些的.
    只能通过一些特别的手段达到一定的效果而已.
    这个组件就是搜索时找到的,主要是通过上传时得到文件一些信息,他主要用作图片上传,
    文件上传时获得相关内容.而且他还能操作摄像头用于头像.
    并且他还有个jquery的插件,主要用处也是上传头像时的相关操作.

 <!--more-->   
    
## 一些经验
    因为不符合我的需求所以没太深入研究,只是测试下了获得上传文件内容.
    把项目下载下来后,把下个文件放在根目录里就可以了.
    还有一点注意,必须放在php环境里,要不会出错.
    
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
	<meta http-equiv="Content-Type" content="text/html; charset=utf-8">

	<meta name="viewport" content="user-scalable=no, width=400, initial-scale=0.8, maximum-scale=0.8" />
	<meta name="apple-mobile-web-app-capable" content="yes" />
	<meta name="apple-mobile-web-app-status-bar-style" content="yes" />
	<meta name="format-detection" content="email=no" />
	<meta name="HandheldFriendly" content="true" />
	<script>
		// Объект настройки
		var FileAPI = {
			debug: true, // режим отладки
			staticPath: './' // путь до флешек
		};
	</script>
	<title>Document</title>
	<script src="dist/FileAPI.min.js"></script>

</head>
<body>
	<input type="file" name='my-input' id='singleFile'>
	<script>

	(function (){
		FileAPI.event.on(singleFile,'change' ,function (evt/**Event*/){
		  // Retrieve file list
		  var files = FileAPI.getFiles(singleFile);

				if( files.length ){
					FileAPI.each(files, function (file){

						FileAPI.readAsText(file, "utf-8", function (evt/**Object*/){
						  if( evt.type == 'load' ){
						    // Success
						     var text = evt.result;
						     //console.log(text);
						     alert(text);
						  } else if( evt.type =='progress' ){
						    var pr = evt.loaded/evt.total * 100;
						    console.log(pr);
						  } 
						});

					});
				}
		});
	})();
	</script>
</body>
</html>
```