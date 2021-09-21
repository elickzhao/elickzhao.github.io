---
title: vue键盘监听事件
tags:
  - js
  - vue
abbrlink: 38944
date: 2017-07-29 21:58:02
categories:
---

**聚焦一个焦点,判断键值这么写**
```html
<html>
<head>
    <meta charset="utf-8">
    <title></title>
    <script src="../js/Vue.js" charset="utf-8"></script>
    <script type="text/javascript">
        window.onload = function () {
            var vm = new Vue({
                el: '#box',
                data: {},
                methods: {
                    show: function (ev) {
                        if(ev.keyCode==13){
                            alert('你按了回车键！')
                        }
                    }
                }
            });
        }
    </script>
</head>
<body>
<div id="box">
    <input type="text" @keyup="show($event)">    <!-- 这里 @keydown 也是可行的-->
</div>
</body>
</html>
```

<!--more--> 

**简单写法**
```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title></title>
    <script src="../js/Vue.js" charset="utf-8"></script>
    <script type="text/javascript">
        window.onload = function () {
            var vm = new Vue({
                el: '#box',
                data: {},
                methods: {
                    show: function () {
                        alert('你按了回车！');
                    },
                    show2: function () {
                        alert('你按了回车！');
                    },
                    show3: function () {
                        alert('你按了上键！');
                    },
                    show4: function () {
                        alert('你按了下键！');
                    },
                    show5: function () {
                        alert('你按了左键！');
                    },
                    show6: function () {
                        alert('你按了右键！');
                    }
                }
            });
        }
    </script>
</head>
<body>
<div id="box">
    <input type="text" @keyup.13="show()">
    <hr>
    <input type="text" @keyup.enter="show2()">
    <hr>
    <input type="text" @keyup.up="show3()">  <!-- 这里是方向键的上下左右 -->
    <hr>
    <input type="text" @keyup.down="show4()">
    <hr>
    <input type="text" @keyup.left="show5()">
    <hr>
    <input type="text" @keyup.right="show6()">
    <hr>
</div>
</body>
</html>
```

<iframe width="100%" height="450" src="http://code.hcharts.cn/test123/BJBlTx/share/result,js,html" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

---

整个页面监听键盘按键
```js
export default {
    mounted: function () {
        // 用 $nextTick 也不行 应该是这里可以用this 但是 document.onkeyup 里面不行
        // this.$nextTick(function () {
        //     // 代码保证 this.$el 在 document 中
        //     document.onkeyup = function (ev) {
        //         if (ev.keyCode == 13) {
        //             this.handleSubmit('formInline');
        //         }
        //     }
        // })
        
        //就是这里了 用钩子在组件加载完毕后 监听按键  这样当页面按下回车就会执行下面方法
        let self = this;
        document.onkeyup = function (ev) {
            if (ev.keyCode == 13) {
                self.handleSubmit('formInline');
            }
        }

    },
    methods: {
        handleSubmit(name) {
            this.$refs[name].validate((valid) => {
                if (valid) {
                    this.$Message.success('登录成功!');
                    this.$router.push({ path: '/main' })

                    //模拟数据测试
                    let mock = new MockAdapter(axios);

                    var template = {
                        'people|1-4': [{
                            'name': '@name',
                            'age': '@integer(10,80)'
                        }]
                    }
                    let data = Mock.mock(template);

                    mock.onGet("www.h.com").reply(200, data);
                    axios.get("www.h.com").then(function (res) {
                        let u = res.data.user;
                        console.log(JSON.stringify(res.data, null, 2));
                    });


                } else {
                    this.$Message.error('表单验证失败!');
                }
            })
        }
    }
}
```


## 参考文档
[键值修饰符](https://cn.vuejs.org/v2/guide/events.html#键值修饰符)
[Vue键盘事件](http://blog.csdn.net/h1069495874/article/details/55225284)
[vue里面的针对window的键盘监听事件，应该怎么写](https://segmentfault.com/q/1010000008744464/a-1020000008745797)



