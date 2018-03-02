---
title: particlesJS 粒子背景的库 使用简介
date: 2017-06-02 23:28:06
tags: [前端,js]
categories:
---


<iframe width="100%" height="450" src="https://code.hcharts.cn/test123/rUMngq/share/result,js,html,css" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

**使用方法:**
```html
<!-- 动画容器 -->
<div id="particles-js"></div>
<!-- 库引入 -->
<script src="particles.js"></script>
<!-- 配置及使用 -->
<script src="app.js"></script>

<style>

/*
这句很关键 如果不设置的话大小总会出现偏移的 不过按照官方的说应该是 #particles-js 也就是容器id 可不知道为啥是canvas类名
还有个问题就是我这个文件不知道从哪里来的,好像跟从官方复制的不一样,把Github上的复制到文件里就报错了
*/
.particles-js-canvas-el {
    position: absolute;
    display: block;
    top: 0;
    left: 0;
    /*z-index:-1;*/ /*有的说这句必须加 要不会阻挡前面内容 不过我这次用的时候没出现 反而会多退后一层*/
}
</style>
```
>注意: 如果下次样式还出现偏移,就按原来官方的试试 使用#particles-js 写样式

<!-- more -->

```js
/* particlesJS.load(@dom-id, @path-json, @callback (optional)); */
/* particles.json 是具体的 配置内容 */
particlesJS.load('particles-js', 'assets/particles.json', function() {
  console.log('callback - particles.js config loaded');
});
```




**配置解说:**
```json
 {	//dom标签
    "particles": {
        "number": { //粒子的数量
            "value": 80,
            "density": {  //粒子的稀密程度 这个轻易别该 会死机 越大越稀疏
                "enable": true,
                "value_area": 1000  // 每一个粒子占据的空间
            }
        },
        "color": {
            "value": ['#2d8cf0', '#19be6b', '#ff9900', '#ed3f14'] //粒子颜色 （支持16进制”#b61924”，rgb”{r:182, g:25, b:36}”，hsl，以及random）['#2d8cf0', '#19be6b', '#ff9900', '#ed3f14']
        },
        "image": {  //如果使用图片 这里设置
            "src": "img/github.svg",
            "width": 100,
            "height": 100
        },
        "shape": {
            "type": "circle", // 粒子的形状 （”circle” “edge” “triangle” “polygon” “star” “image”）
            "stroke": { // 粒子边缘线条 宽度和颜色
                "width": 0,
                "color": "#000000"
            },
            "polygon": {  //多变形状,不太理解
                "nb_sides": 5
            },
        },
        "opacity": {
            "value": 0.75,  //粒子透明度 (0~1)
            "random": true,  //随机透明度
            "anim": {
                "enable": false,  //开启动画 这样粒子的透明度将不断变化
                "speed": 1,     //数值越大 速度变化越快
                "opacity_min": 0.1, //应该是透明变化幅度
                "sync": false
            }
        },
        "size": {
            "value": 10,  //粒子大小
            "random": true,
            "anim": {
                "enable": false, //大小动画
                "speed": 40,
                "size_min": 0.1,
                "sync": false
            }
        },
        "line_linked": {  //连线
            "enable": true,
            "distance": 150,  //相距多少连线
            "color": "#ffffff",
            "opacity": 0.4, //透明度
            "width": 1
        },
        "move": {
            "enable": true, //如果关闭了 就没有画面了
            "speed": 4,   //越大速度越快
            "direction": "none", //移动方向 none碰撞运动
            "random": false,   //粒子随机运动了 也没物理特性了
            "straight": false,   //静止 
            "out_mode": "out",  //粒子是否可以出边缘 out/bounce
            "bounce": false,      //粒子之间是否碰撞
            "attract": {       //这个不知道是干啥的 好像粒子互相吸引
                "enable": false,
                "rotateX": 600,
                "rotateY": 1200
            }
        }
    },
    "interactivity": {
        "detect_on": "canvas",
        "events": {
            "onhover": {  //鼠标是否互动
                "enable": false,
                "mode": "repulse" //互动模式 ["grab", "bubble","repulse"] 第一个是连线 第二个粒子放大  第三个是推开
            },
            "onclick": {
                "enable": false,
                "mode": "repulse"  //点击特效 "push" ["push", "remove","bubble" ,"repulse"] push是添加 其他的和上面一样
            },
            "resize": true  //重画不太懂干什么用的
        },
        "modes": {  //这里是模式的设置
            "grab": {
                "distance": 140,
                "line_linked": {
                    "opacity": 1
                }
            },
            "bubble": {
                "distance": 400,
                "size": 40,
                "duration": 2,
                "opacity": 8,
                "speed": 3
            },
            "repulse": {
                "distance": 200,
                "duration": 0.4
            },
            "push": {
                "particles_nb": 4
            },
            "remove": {
                "particles_nb": 2
            }
        }
    },
    "retina_detect": true //不知道干啥的
}
```


**例子:**
自己的
<iframe width="100%" height="450" src="https://code.hcharts.cn/test123/rUMngq/share/result,js,html,css" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

星空
<iframe width="100%" height="450" src="https://code.hcharts.cn/test123/rUMngq/2/share/result,js,html,css" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

经典
<iframe width="100%" height="450" src="https://code.hcharts.cn/test123/rUMngq/1/share/result,js,html,css" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

泡泡版
<iframe width="100%" height="450" src="https://code.hcharts.cn/test123/rUMngq/3/share/result,js,html,css" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

##参考文档
[Github仓库](https://github.com/VincentGarreau/particles.js)
[particlesJS使用简介](http://blog.csdn.net/csdn_yudong/article/details/53128570)

**这个是个整合 其配置比上面简单很多  但本质还是上面的particles**
[jparticles.js](https://jparticles.js.org/#/)