---
title: Swagger简介
date: 2017-08-25 17:41:59
tags: [js,node.js]
categories:
---

主要看这篇文章就行了 [Swagger从入门到精通](https://www.gitbook.com/book/huangwenchao/swagger/details) 虽然可能有些地方可能有老了,没讲到不过还是很实用,而且非常细致.

剩下的看编辑器事例就好了,一下子就能看懂了 [官方在线编辑器](http://editor.swagger.io/)


**记录点一:**

当请求操作时,有两种方式一种是路径请求,另一种是参数请求.两种表现形式不同.

**参数请求**
```js
// get(请求方式) /persons?pageSize=20&pageNumber=2
      parameters:
       - name: pageSize
         in: query      //就是这里,这里query表示参数请求
         description: Number of persons returned
         type: integer
       - name: pageNumber
         in: query
         description: Page number
         type: integer
```

**路径请求**
```js
// get(请求方式) /persons/{username}
      parameters:
        - name: username
          in: path          //这里path表示路径请求
          required: true    //这个表示路径参数必须填写,默认为false表示这个路径是可选填的
          description: The person's username
          type: string
```

<!--more-->

**记录点二**
返回数据形式

```js
      responses:
        '200':
          description: successful operation
          schema:
            type: array
            items:
              $ref: '#/definitions/Pet' //这个是连接底下的数据结构
              
              ....
              

//其实这就是数据库的结构              
 Pet:
    type: object
    required:
      - name
      - photoUrls
    properties:
      id:
        type: integer
        format: int64
      category:
        $ref: '#/definitions/Category'  //这个还可以关联另一个数据表内容 这样返回时很简单.
      name:
        type: string
        example: doggie                 //这是事例 在测试时就直接显示了
      photoUrls:
        type: array
        xml:                            // 这个xml可能是在选择返回为xml才有用 但一般是json格式 
          name: photoUrl
          wrapped: true
        items:                          //这个才是json正常显示的东西
          type: string
      tags:
        type: array
        xml:
          name: tag
          wrapped: true
        items:
          $ref: '#/definitions/Tag'
      status:
        type: string
        description: pet status in the store
        enum:
          - available
          - pending
          - sold
    xml:
      name: Pet              
```

**记录点三**
 定义消息体参数
 接下来我们给 post 方法添加参数，通过 in 属性显式说明参数是在 body 中的。参数的定义参考 get /persons/{username} 的 200 响应消息体参数，也就是包含用户的姓氏、名字、用户名。
```js
      parameters:
        - name: person
          in: body
          description: The person to create.
          schema:
            required:
              - username
            properties:
              firstName:
                type: string
              lastName:
                type: string
              username:
                type: string
```

[官方在线编辑器](http://editor.swagger.io/)
[Swagger - 前后端分离后的契约](http://www.cnblogs.com/whitewolf/p/4686154.html)
[[译]5.41 Swagger tutorial](http://www.cnblogs.com/JoiT/p/6378086.html)
[Swagger从入门到精通](https://www.gitbook.com/book/huangwenchao/swagger/details)
[Swagger](http://www.jianshu.com/p/4115f2b53983)

