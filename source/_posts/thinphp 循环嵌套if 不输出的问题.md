---
title: thinphp 循环嵌套if 不输出的问题
date: 2018-02-08 22:16:17
tags: [php,小百科]
categories:
---
这个以前应该是遇到过,不过很久不用都忘记了.这次就记录下吧.免得以后没处查去.

原因很简单 当双重循环 **volist** 的时候, 如果做两重循环的判断.
这是就不能用简写的 `$vo.id` 这种形式了 必须用数组的形式 `$vo['id']`
这样就ok了

```html
<select class="lay-search" name="cateid" id="cateid" lay-filter="cid_1" lay-search lay-verify="cateid">
    <option value>所有分类</option>
    <option value="">请选择</option>
    <volist name="cate_list" id="v">
        <if condition="$v.tid eq 1">
            <optgroup label="{$v.name}">
                <volist name="cate_list" id="vo">
                <!-- 注意这里 这里用的是数组形式 -->
                    <if condition="$vo['tid'] == $v['id']">
                        <option value="{$vo.id}">{$vo.name}</option>
                    </if>
                </volist>
            </optgroup>
        </if>
    </volist>
</select>
```
