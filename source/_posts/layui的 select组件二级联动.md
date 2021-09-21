---
title: layui的 select组件二级联动
tags:
  - 前端
  - JavaScript
  - css
abbrlink: 53693
date: 2018-02-01 11:52:10
categories:
---

**我自己的 因为和h-ui合用 所以有点问题 一些网上的写法不奏效**

```js
var form = layui.form;
form.on('select(cid_1)', function (data) {
    var cateid = $('#cateid').val();
    $("cid").val(cateid); 
    $.post('{:U("getcid")}', {
        cateid: cateid,
        async :false,
    }, function (data) {
        if (data.catelist != '') {
            var htmls = '<option value="">二级分类</option>';
            var cate = data.catelist;
            for (var i = 0; i < cate.length; i++) {
                htmls += '<option value="' + cate[i].id + '">-- ' + cate[i].name + '</option>';
            }
            $('#cid').html(htmls);
            $('#catedesc').html('&nbsp;&nbsp; * 必选项');
            form.render();  //这里只有这么写才可以 看来某些东西冲突了
            //form.render();// form.render('select', 'cid_2');  //其他渲染方式都无法渲染出内容来 虽然 select 里已经有东西了

        } else {
            $('#cid').html('<option value="">二级分类</option>');
            $('#catedesc').html('&nbsp;&nbsp; * 该分类下还没有二级分类，请先添加');
            form.render();
        }
    }, "json");
```


----
**这是别人比较简洁的写法**
```js
//下拉框  
var form = layui.form();  
//lay-filter="mch_id" 的值。 select()里的参数就是这个  lay-filter是放在select里的一个属性
form.on('select(mch_id)', function(data) {  
    var mch_id = data.value;  
    $.ajax({  
        type:"POST",  
        url:"{:url('get_store')}",  
        dataType:"json",  
        data:{'mch_id':mch_id},  
        success:function(e){  
            //empty() 方法从被选元素移除所有内容  
            $("select[name='store_id']").empty();  
            var html = "<option value='0'>请选择所属门店</option>";  
            $(e).each(function (v, k) {  
                html += "<option value='" + k.id + "'>" + k.username + "</option>";  
            });  
            //把遍历的数据放到select表里面  
            $("select[name='store_id']").append(html);  
            //从新刷新了一下下拉框  
            form.render('select');      //重新渲染
        }  
    })  
}) 
```


## 参考文档
[基于 Layui form 组件的省市区级联的实现](http://fly.layui.com/jie/4875/)
[tp5与layui框架实现二级联动加分页效果](http://blog.csdn.net/weixin_39973810/article/details/78852130)
