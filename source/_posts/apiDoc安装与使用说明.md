---
title: apidoc安装与使用说明
tags:
  - js
  - npm
abbrlink: 46578
date: 2016-10-28 22:31:25
categories:
---
安装很简单,全局安装这样就可以使用 **apidoc** 的命令了
`npm install apidoc -g `

使用
- 首先必须在项目根目录建立个 `apidoc.json` 的配置文件.里面大概这么写.
```json
{
    "name": "lumen-api-demo",
    "version": "0.1.0",
    "description": "LUMEN API DEMO",
    "title": "lumen api demo",
    "url" : "http://jwt.cc/api"
}
```

- 使用命令生成文档文件 `apidoc -i  app/Http/Controllers/Api/V1 -o public/apidoc`

-  `@apiSuccess (Reponse 200) {json} [data='""'] 如果有数据返回` []就是可选项在表格显示中有提示

- 多个版本很简单 只要把注释里的`@apiVersion 0.1.0`修改一下就可以了. 使用生成命令时,记得一定要在版本的上一级目录才可以.`apidoc -i  app/Http/Controllers/Api -o public/apidoc` 这里Api目录下面有 v1和v2两个目录.还有记得修改`apidoc.json`里的 `version` 要不默认显示的还是老版本

## 参数解释

```
@api {method} path [title]
  只有使用@api标注的注释块才会在解析之后生成文档，title会被解析为导航菜单(@apiGroup)下的小菜单
  method可以有空格，如{POST GET}
@apiGroup name
  分组名称，被解析为导航栏菜单
@apiName name
  接口名称，在同一个@apiGroup下，名称相同的@api通过@apiVersion区分，否者后面@api会覆盖前面定义的@api
@apiDescription text
  接口描述，支持html语法
@apiVersion verison
  接口版本，major.minor.patch的形式

@apiIgnore [hint]
  apidoc会忽略使用@apiIgnore标注的接口，hint为描述
@apiSampleRequest url
  接口测试地址以供测试，发送请求时，@api method必须为POST/GET等其中一种

@apiDefine name [title] [description]
  定义一个注释块(不包含@api)，配合@apiUse使用可以引入注释块
  在@apiDefine内部不可以使用@apiUse
@apiUse name
  引入一个@apiDefine的注释块

@apiParam [(group)] [{type}] [field=defaultValue] [description]
@apiHeader [(group)] [{type}] [field=defaultValue] [description]
@apiError [(group)] [{type}] field [description]
@apiSuccess [(group)] [{type}] field [description]
  用法基本类似，分别描述请求参数、头部，响应错误和成功
  group表示参数的分组，type表示类型(不能有空格)，入参可以定义默认值(不能有空格)
@apiParamExample [{type}] [title] example
@apiHeaderExample [{type}] [title] example
@apiErrorExample [{type}] [title] example
@apiSuccessExample [{type}] [title] example
  用法完全一致，但是type表示的是example的语言类型
  example书写成什么样就会解析成什么样，所以最好是书写的时候注意格式化，(许多编辑器都有列模式，可以使用列模式快速对代码添加*号)

@apiPermission name
  name必须独一无二，描述@api的访问权限，如admin/anyone
```


## 参考文档
[官方网站](http://apidocjs.com/#demo)
[Web API文档生成工具apidoc](http://www.jianshu.com/p/bb5a4f5e588a)
