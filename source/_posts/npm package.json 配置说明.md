---
title: npm package.json 配置说明
date: 2016-05-08 23:06:01
tags: npm
categories: 前端技术
---



```json
{
  "name": "studyangular",
  "version": "0.0.0",
  "dependencies": {},
  "repository": {},
  "devDependencies": {
    "grunt": "^0.4.5",
    "grunt-autoprefixer": "^2.0.0",
    "grunt-concurrent": "^1.0.0",
    "grunt-contrib-clean": "^0.6.0",
    "grunt-contrib-compass": "^1.0.0",
    "grunt-contrib-concat": "^0.5.0",
    "grunt-contrib-connect": "^0.9.0",
    "grunt-contrib-copy": "^0.7.0",
    "grunt-contrib-cssmin": "^0.12.0",
    "grunt-contrib-htmlmin": "^0.4.0",
    "grunt-contrib-imagemin": "^0.9.2",
    "grunt-contrib-jshint": "^0.11.0",
    "grunt-contrib-uglify": "^0.7.0",
    "grunt-contrib-watch": "^0.6.1",
    "grunt-filerev": "^2.1.2",
    "grunt-google-cdn": "^0.4.3",
    "grunt-newer": "^1.1.0",
    "grunt-ng-annotate": "^0.9.2",
    "grunt-svgmin": "^2.0.0",
    "grunt-usemin": "^3.0.0",
    "grunt-wiredep": "^2.0.0",
    "jshint-stylish": "^1.0.0",
    "load-grunt-tasks": "^3.1.0",
    "time-grunt": "^1.0.0"
  },
  "engines": {
    "node": ">=0.10.0"
  },
  "scripts": {
    "test": "grunt test"
  }
}
```

**Private**
最喜欢这个字段 设置为true下面那些就全部用写了,开发时 或者 不想共享时 就用这个
可选字段，布尔值。如果private为true，npm会拒绝发布。这可以防止私有repositories不小心被发布出去。

<!--more-->

**Name**

必须字段,包名称

小提示：

 - 不要在name中包含js, node字样；
 - 这个名字最终会是URL的一部分，命令行的参数，目录名，所以不能以点号或下划线开头；
 - 这个名字可能在require()方法中被调用，所以应该尽可能短；

**Version**
必须字段,包版本
name和version是package.json中最重要的两个字段，也是发布到NPM平台上的唯一标识，如果没有正确设置这两个字段，包就不能发布和被下载。新版本的NPM可以指定scope, 名字可以加前缀标识，如@ijse/mypackage。

**Description**

包的描述信息，可选字段，必须是字符串。 npm search 的时候会用到。

**Keywords**

包的关键词信息，是一个字符串数组， npm search 的时候会用到。

**Homepage**

包的主页地址,可选字段， 没有 http:// 等带协议前缀的URL。

**bugs**
可选字段，问题追踪系统的URL或邮箱地址； npm bugs 用的上。包的bug跟踪主页地址，应该如下设置
```json
{ "url" :"http://github.com/owner/project/issues",

"email" :"project@hostname.com"

}
```

**License**

包的开源协议名称,可选字段。

如果是使用一个普遍的license，比如BSD-3-Clause或MIT，直接使用：
```json
{ "license" : "BSD-3-Clause" }
```

**Author , contributors**

都是可选字段。author是一个人，contributors是一组人。

Author的格式如下：
```json
{ "name" : "Barney Rubble",
 "email" : "b@rubble.com",
 "url" : "http://barnyrubble.tumblr.com/"
}
```
或者:
```json
"Barney Rubble <b@rubble.com> (http://barnyrubble.tumblr.com/)"
```

**files**
可选字段，项目包含的一组文件。如果是文件夹，文件夹下的文件也会被包含。如果需要把某些文件不包含在项目中，添加一个”.npmignore”文件。这个文件和”gitignore”类似。

**main**
可选字段。包的入口文件，如index.js 这个字段的值是你程序主入口 模块的ID  。如果其他用户需要你的包，当用户调用require()方法时，返回的就是这个模块的导出（ exports ）。

**bin**
可选字段。很多的包都会有执行文件需要安装到PATH中去。

这个字段对应的是一个Map，每个元素对应一个 { 命令名：文件名 } 。
```json
{ "bin" : { "npm" : "./cli.js" } }
```
当包被安装后，NPM将创建一个cli.js文件的链接到/usr/local/bin/iapp下。

**man**
为系统的man命令提供帮助文档, 如：
```json
"man": "./man/doc.1"
```
帮助文件的文件名必须以数字结尾，如果是压缩的，需要以.gz结尾。

如果是字符串数组：
```json
"name": "foo",
"man": ["./man/foo.1", "./man/bar.1", "./man/foo.2" ]
```
则分别可以man foo, man foo-bar, man 2 foo来查看。

**Directories**

CommonJS包所要求的目录结构信息，目前除了告诉别人你的程序目录结构，貌似没有别的什么用。 
下级字段可以是：lib, bin, man, doc, example。 每个都是字符串

用于指示包的目录结构：

Directories.lib

指示库文件的位置。

Directories.bin

和前面的bin是一样的，但如果前面已经有bin，那么这个就无效。

除了以上两个，还有Directories.doc& Directories.man & Directories.example。

**Repository**

可选字段。用于指示代码存放的位置。
```json
"repository" :
  { "type" : "git"
  , "url" : "http://github.com/npm/npm.git"
  }
"repository" :
  { "type" : "svn"
  , "url" : "http://v8.googlecode.com/svn/trunk/"
  }
```  
Scripts

通过设置这个可以使NPM调用一些命令脚本，封装一些功能。可选字段，object。Key是生命周期事件名，value是在事件点要跑的命令。参考npm-scripts。

**config**
可选字段，object。
添加一些设置，可以供scripts读取用，同时这里的值也会被添加到系统的环境变量中。
Config对象中的值在 Scripts 的整个周期中皆可用，专门用于给Scripts提供配置参数。
```json
"name": "foo",
"config": {
  "port": "8080"
}
```

**Dependencies**
指定依赖的其它包，这些依赖是指包发布后正常执行时所需要的，如果是开发中依赖的包，可以在devDependencies设置。

通常使用下面命令来安装：
```json
npm install --save otherpackage  
```

可选字段，指示当前包所依赖的其他包。
```json
{ "dependencies" :
  { "foo" : "1.0.0 - 2.9999.9999"
  , "bar" : ">=1.0.2 <2.1.2"
  , "baz" : ">1.0.2 <=2.3.4"
  , "boo" : "2.0.1"
  , "qux" : "<1.0.0 || >=2.3.1 <2.4.5 || >=2.5.2 <3.0.0"
  , "asd" : "http://asdf.com/asdf.tar.gz"
  , "til" : "~1.2"
  , "elf" : "~1.2.3"
  , "two" : "2.x"
  , "thr" : "3.3.x"
  }
}
```
版本格式可以是下面任一种：

- version  完全匹配
- >version  大于这个版本
- >=version 大于或等于这个版本
- <version
- <=version
- ~version  非常接近这个版本
- ^version  与当前版本兼容
- 1.2.x  X代表任意数字，因此1.2.1, 1.2.3等都可以
- http://...  Unix系统下使用的tarball的URL。
- *  任何版本都可以
- "" 任何版本都可以
- version1 - version2   等价于  >=version1 <=version2 .
- range1 || range2  满足任意一个即可
- git...  Git地址
- user/repo
- devDependencies

Git URL可以有如下种形式：
```json
git://github.com/user/project.git#commit-ish  
git+ssh://user@hostname:project.git#commit-ish  
git+ssh://user@hostname/project.git#commit-ish  
git+http://user@hostname/project/blah.git#commit-ish  
git+https://user@hostname/project/blah.git#commit-ish  
```

**devDependencies**

可选字段。如果只需要下载使用某些模块，而不下载这些模块的测试和文档框架，放在这个下面比较不错。
这些依赖只有在开发时候才需要。
```json
npm install --save-dev mypack  
```

**peerDependencies**

可选字段。兼容性依赖。如果你的包是插件，适合这种方式。
相关的依赖，如果你的包是插件，而用户在使用你的包时候，通常也会需要这些依赖（插件），那么可以将依赖列到这里。
```json
"peerDependencies": {
  "karma-jasmine": "~0.1.0",
  "karma-requirejs": "~0.2.0",
  "karma-coffee-preprocessor": "~0.1.0",
  "karma-html2js-preprocessor": "~0.1.0",
  "karma-chrome-launcher": "~0.1.0",
  "karma-firefox-launcher": "~0.1.0",
  "karma-phantomjs-launcher": "~0.1.0",
  "karma-script-launcher": "~0.1.0"
}
```
这些都是karma的相关插件，一般使用karma的时候都会需要。

**bundledDependencies**

可选字段。发布包时同时打包的其他依赖。

**optionalDependencies**

可选字段。如果你想在某些依赖即使没有找到，或则安装失败的情况下，npm都继续执行。那么这些依赖适合放在这里。

**Engines**

可选字段。既可以指定node版本：
```json
{ "engines" : {"node" : ">=0.10.3 <0.12" } }
```
也可以指定npm版本：
```json
{ "engines" : {"npm" : "~1.0.20" } }
```
**engineStrick**

可选字段，布尔值。如果你肯定你的程序只能在制定的engine上运行，设置为true。

**os**
可选字段。指定模块可以在什么操作系统上运行：
```json
"os" : [ "darwin","linux" ]

"os" : [ "!win32" ]
```

**cpu**
可选字段。指定CPU型号。
```json
"cpu" : [ "x64","ia32" ]

"cpu" : [ "!arm","!mips" ]
```
**preferGlobal**

可选字段，布尔值。如果你的包是个命令行应用程序，需要全局安装，就可以设为true。


**publishConfig**

可选字段。发布时使用的配置值放这。
这个字段用于设置发布时候的一些设定。尤其方便你希望发布前设定指定的tag或registry。

也可以设定其它子字段，但只有tag和registry会影响到发布。

**NPM的一些默认值说明**
```json
"scripts":{"start": "node server.js"}  #如果你的包里有server.js文件，npm默认将执行： node server.js 

"scripts":{"preinstall":"node-gyp rebuild"} #如果包里有 binding.gyp， npm默认在 preinstall命令时，使用node-gyp做编译
```

```json
"contributors": [...] #如果项目根目录下含有AUTHORS文件，则NPM会自动将每一行以Name <email> (url)的格式读取并设定此字段。
```




参考文章
[npm package.json 配置文件](http://www.tuicool.com/articles/2uEjqmM)
[package.json for NPM 文件详解](http://ju.outofmemory.cn/entry/130809)