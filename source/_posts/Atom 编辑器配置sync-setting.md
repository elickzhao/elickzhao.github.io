---
title: Atom 编辑器配置sync-setting
tags:
  - atom
  - Git
  - 小百科
  - IDE
categories: 前端技术
abbrlink: 33737
date: 2016-10-18 22:49:16
---

## sync-settings简介

sync-settings是一款备份插件，可以备份ATOM的全局设置、插件、按键绑定(keymaps)、界面样式、代码片段(snippets )、 init script。这样不管走到那里使用的Atom都是自己最熟悉的那个,这样省去了很多麻烦.

<!--more-->

## sync-settings配置

- 申请生成token和gist id
    -  申请goken
    ![](http://img.blog.csdn.net/20160712181041681)
    - Select scopes 记住一定要把`gist`打上勾.要不无法找到 `gist`
    ![](/image/16-10/1.png)
    - 创建新的gist
        - 登陆github，点击Gist图标(使用gist功能需要翻墙)：
         ![](http://img.blog.csdn.net/20160712174133844)
        - 创建个gist项目,这个项目是私人或公开的都可以
          ![](http://img.blog.csdn.net/20160712174836003)
        - 成功创建后将会跳转到刚刚创建的gist项目浏览页面，url处昵称后面的字符即为gist id，粘贴到sync-settings的配置处即可
          ![](http://img.blog.csdn.net/20160712175604129)

- 设置sync-setting
  ![](http://img.blog.csdn.net/20160712173400621)

## 测试下是否正常

- 在文档编辑页面,按下全局命令搜索面板(Ctrl+shift+p)
- 搜索sync- ,会有可选项
   - sync-settings:backup – 这条命令是备份当前的配置
   - sync-settings:restore – 这条命令是回复配置,是直接覆盖的;
   - sync-settings:view-backup – 这条是当你执行备份后到线上查询你的备份的,也就是到你的gist code里面
   - sync-settings:check-backup – 这条是查询最后一次是否正常

备份成功和失败都有一条信息提醒,看图.
![](http://img3.07net01.com/upload/images/2015/08/05/1647021050918243.png)

## 参考文档
[Atom编辑器折腾记_(12)Sync-setttings(插件-备份神器)](http://www.07net01.com/2015/08/893825.html)
[ATOM基础教程一sync-settings配置(11)](http://blog.csdn.net/zsl10/article/details/51891306)
