---
title: gulp学习
date: 2016-05-09 00:19:49
tags: [npm,gulp]
categories: 前端技术
---

简单说下gulp使用流程

- 首先进入 项目目录  npm install --save-dev gulp
- 其次安装其他所需插件 配置文件里有 就不赘述了
- 规划目录开始编写

[项目地址](https://code.csdn.net/xwiwi/gulp-learning.git)

```
gulp #目录结构
├─.idea
│  └─libraries
├─app
│  └─assets     # 随便找的测试例子
│      ├─css
│      │  └─font
│      ├─images
│      └─js
├─build         # 第一个实例 输出目录
├─dist          # 第二个实例 输出目录
├─js            # 第一个实例 随便写的js
└─web           # browser-syn 测试 随便写的scss     
    ├─css
    └─scss
```

**Note:**
因为gulp-sass需要node-sass的支持才能用,不过node-sass被墙住了,所以这里共享一个,不过版本是3.7的 如果升级了就自行更换吧
https://yunpan.cn/cPI3SEfajJeX9  访问密码 a6b0

<!--more-->

```js
/**
 * Created by elick on 2016/5/8.
 */
/* 这是packages.json的配置 不过里面不能写注释所以写在这里了
"devDependencies": {              //常用插件
        "browser-sync": "^2.12.5",      //浏览器同步插件
        "gulp": "^3.9.1",
        "gulp-autoprefixer": "^3.1.0",  //使用Autoprefixer来补全浏览器兼容的css
        "gulp-concat": "^2.6.0",        //合并js
        "gulp-jshint": "^2.0.0",        //检测js有没有报错或警告
        "jshint": "^2.9.2",             // gulp-jshint 不能单独使用 需要配合这个
        "gulp-load-plugins": "^1.2.2",  //读取插件 不用每个都都定义变量了
        "gulp-rename": "^1.2.2",        //改写名字
        "gulp-sass": "^2.3.1",          //解析sass文件
        "node-sass": "^3.7.0"           //gulp-sass不能单独使用需要配合这个 //这个需要翻墙才能npm下载
        "gulp-uglify": "^1.5.3"         //压缩文件
}
*/


var gulp = require('gulp'),
    uglify = require('gulp-uglify'),
    browserSync = require('browser-sync');

//这个插件的意义就在上面 不用去写那么多注册 这一行就可以搞定了
var gulpLoadPlugins = require('gulp-load-plugins'),
    plugins = gulpLoadPlugins();

gulp.task('minify',function(){
    gulp.src('js/app.js')
        .pipe(uglify())
        .pipe(gulp.dest('build'))
});

gulp.task('js',function(){
    return gulp.src('js/*.js')
        .pipe(uglify())
        .pipe(plugins.concat('app.js'))  //这就是那个plugins插件的用法 不需要require(concat)
        .pipe(gulp.dest('build'));       //输出目录
});

gulp.task('watch',function(){
    var watcher = gulp.watch('templates/*.tmpl.html', ['build']);  //watch() 这个函数自带的 不需要插件
    watcher.on('change', function (event) {
        console.log('Event type: ' + event.type); // added, changed, or deleted
        console.log('Event path: ' + event.path); // The path of the modified file
    });

    //与上面的watcher.on的功能等同
    //gulp.watch('templates/*.tmpl.html', function (event) {
    //    console.log('Event type: ' + event.type); // added, changed, or deleted
    //    console.log('Event path: ' + event.path); // The path of the modified file
    //});
});



// 另一篇文章的插件测试 ---------------------------------------

//引入gulp
var gulp = require('gulp');

//引入组件
var jshint = require('gulp-jshint');    //这个还得配合jshint使用,单独使用不可以
var sass = require('gulp-sass');
var concat = require('gulp-concat');
var uglify = require('gulp-uglify');
var rename = require('gulp-rename');    //经过测试这个组件基本毫无意义 也许有用我现在没发现

//其实下面可以写在一起 不过这样分开写不错 因为可以单独执行

//检查脚本
gulp.task('lint',function(){
    gulp.src('./js/*.js')
        .pipe(jshint())
        .pipe(jshint.reporter('default'));
});

// 编译Sass
gulp.task('sass', function() {
    gulp.src('./scss/*.scss')
        .pipe(sass())
        .pipe(gulp.dest('./css'));
});

//合并压缩文件
gulp.task('scripts',function(){
    gulp.src('./js/*.js')
        .pipe(uglify())
        .pipe(concat('all.min.js'))
        .pipe(gulp.dest('./dist'));
});

//默认任务
gulp.task('default',function(){
    gulp.run('lint','sass','scripts'); //运行任务

    //监听文件变化
    gulp.watch('./js/*.js',function(){
        gulp.run('lint','sass','scripts');
    });
});




// browser-sync 测试 --------------------------------------------------------------


//这种配置是静态文件配置方法
// 不对 其实把less或者sass也放到监控里其实一样
// 还是错了 还是静态的 因为less和sass需要 gulp转换才可以 目前这里并没有转换任务
//因为单一执行的browser-sync一个任务
//https://github.com/BrowserSync/gulp-browser-sync 这里有很多配置方法的例子
gulp.task('browser-sync',function(){
    var files = [
        'app/**/*.html',
        'app/assets/css/**/*.css',
        'app/assets/images/** /*.png',
        'app/assets/js/**/*.js'
    ];

    browserSync.init(files,{
        server: {
            baseDir: './app/assets' //这个是主页地址
        }
    });
});


//第二个测试 ----------------------------------------------------------------

var filter = require('gulp-filter');
var reload = browserSync.reload;

gulp.task('b-sync',function(){
    //第二种 用watch监控因为 wtach可以写多个  这个服务器写法就得和下面gulp.watch('web/*.html')配合
    //要不只能监控sass变化了
    browserSync({
        server:{
            baseDir: "./web"
        }
    });

    //第一种写法 用browserSync 来监控文件
    //这么写也可以啊  而且能监控html文件变化 上面那个只能监控sass文件
    //var files = [
    //    'web/**/*.html',
    //    //'web/**/*.css', //用上这句 其实下面sass任务里的 reload可以去掉 不过速度比那个慢很多所以还是把这个去掉吧
    //
    //];
    //
    //browserSync.init(files,{
    //    server: {
    //        baseDir: './web' //这个是主页地址
    //    }
    //});

});


gulp.task('sass',function(){
    return gulp.src('web/scss/styles.scss')
        .pipe(sass({includePaths:['web/scss']})) //编译SASS
        .pipe(gulp.dest('web/css')) // 写入CSS目录
        .pipe(filter('web/**/*.css')) //过滤流确保CSS文件通过。
        .pipe(reload({stream:true})); //注入浏览器
});

gulp.task('def',['sass','b-sync'],function(){
    gulp.watch('web/scss/*.scss',['sass']);
    gulp.watch('web/*.html').on('change',reload);
});
```


```json
{
  "private": true,
  "engines": {
    "node": ">=0.12.0"
  },
  "devDependencies": {
    "browser-sync": "^2.12.5",
    "gulp": "^3.9.1",
    "gulp-concat": "^2.6.0",
    "gulp-jshint": "^2.0.0",
    "gulp-load-plugins": "^1.2.2",
    "gulp-uglify": "^1.5.3"
  }
}

```

参考文章:
[Gulp开发教程：Building With Gulp](http://www.tuicool.com/articles/AzI3Ib)
[前端构建工具gulp入门教程](https://segmentfault.com/a/1190000000372547)
[前端构建之gulp与常用插件](http://www.mamicode.com/info-detail-517085.html)
[Browsersync + Gulp](https://github.com/BrowserSync/gulp-browser-sync)