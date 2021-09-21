---
title: layui表单验证 自定义验证 select验证方法
tags:
  - 前端
  - css
  - js
abbrlink: 30851
date: 2018-02-04 14:28:43
categories:
---

这个东西还真是浪费了不少时间,网上一些文章写的吧 不知道是原来版本那么做 还是以讹传讹 反正都不太好用
只能大致参考下  好了 进入整体

```html
<!--  注意这里 lay-filter 是监听时 或者其他操作时用的 选取用的字段 类似jq的 $('#id')-->
<!--  注意这里 lay-verify 是你自定义的验证规则名称 这个是必须 如果是简单验证 就写上预设值 就好了 -->
<select name="city" lay-filter="city" lay-verify="city-verify">
  <option value="010">北京</option>
  <option value="021" disabled>上海（禁用效果）</option>
  <option value="0571" selected>杭州</option>
</select>    

<!-- 可以是submit提交按钮 也可以是普通按钮 但是呢 必须加上 lay-submit lay-filter="*" 其中 lay-submit 也是不可少的  要不就检测不到提交事件-->
<input type="submit" name="submit" lay-submit lay-filter="*" value="提交">

<script>
//第一步 写自定义规则
form.verify({
    cateid: function (value) {
        if (value == "") {
            return "必须选择一级栏目";
        }
    },
    contact: function (value) {
        if (value.length < 4) {
            return '内容请输入至少4个字符';
        }
    },
    phone: [/^1[3|4|5|7|8]\d{9}$/, '手机必须11位，只能是数字！'],
    email: [/^[a-z0-9._%-]+@([a-z0-9-]+\.)+[a-z]{2,4}$|^1[3|4|5|7|8]\d{9}$/, '邮箱格式不对']
});

//第二部 提交监听事件
//只有这样 验证才会执行  submit(*) 就是提交按钮的 lay-filter="*" 这个可以自行更改
//其实回调函数 只是有其他操作要做的时候用的 如果只是验证的话 第二个参数可以不写
form.on('submit(*)', function (data) {
    params = data.field;
    //alert(JSON.stringify(params))
    submit($, params);  //这个是提交按钮 当验证通过后就提交
    return false;
})
//其实可以直接这么用
//form.on('submit(*)');
</script>

```

<!--more-->

---

**唠叨几句**

原本没打算用 **layui** 的验证,原本是用 **jquery.validate** ,可是因为 **layui** 改变了select外观 
所以根本检测不到. 这里顺便把 **jquery.validate** 也简单说一下吧

```js
$(function () {
    $("#commentForm").validate({
        rules: {    //这一定不要忘记 有个 rules 对象 而不是把验证规则直接放里
            name: {
                minlength: "2",
                required: true
            },
            cateid: {
                required: true
            },
            cid: {
                required: true,
            }
        },
        onkeyup: false,
        //focusCleanup:true //当错误元素,获得焦点时,就把错误提示去掉.这个不太好,应该是失去焦点在去掉
    });
});
```

## 参考文档
[文本框，手机，邮箱，textarea等格式的验证](https://www.zybuluo.com/mdeditor#1038324)
