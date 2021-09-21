---
title: Dingo Transformers 的使用(Fractal)
tags: laravel dingo
abbrlink: 42699
date: 2016-12-18 22:47:45
categories:
---

**Post**
```php
    public function index($postId)
    {
        $post = $this->postRepository->find($postId);

        if (! $post) {
            return $this->response->errorNotFound();
        }

        // 研究一下cursor，这里应该无限下拉
        $comments = $this->postCommentRepository
            ->where(['post_id' => $postId])
            ->paginate();

        return $this->response->paginator($comments, new PostCommentTransformer());
    }
```

>这里主要需要说明的是, include的用法. include 的作用是,把附加信息带上.
比如你要查询post的评论.你只需要 `http://jwt.cc/api/posts?include=user` 带上个参数就可以了.
而不需要多加个路由. 这个参数不是随便起的,必须先定义.
这个方便是方便,不过还是有点疑问.就是缓存需要怎么做呢.所以这次追求速度就把这个方法舍弃了
还有就是带参数那个不知道怎么请求 `public function includeComments(Post $post, ParamBag $params = null)`

----
**补充:**
最近又看了一遍,又有了更全面的认识.他这里做到的表关联,其实还是用的laravel表关联. `$post->user`其实就是表关联方法,就在model里.
经过测试 include 必须是关联表,因为我用 return 返回原表数据格式报错了.
看来一个表只能一种转换格式,不能有多个格式,这是问题啊.比如我一个页面有图片字段,另一个页面没有.
就不能通过多个格式返回来操作,只能是舍弃多的那个字段,不过要是区别很大的话就还是比较麻烦.

**PostTransformers**
```php

namespace ApiDemo\Transformers;

use ApiDemo\Models\Post;
use League\Fractal\TransformerAbstract;

class PostTransformer extends TransformerAbstract
{

    // 在这里定义 include
    protected $availableIncludes = ['user', 'comments'];

    public function transform(Post $post)
    {
        return $post->attributesToArray();
    }
    //请求方式
    //http://jwt.cc/api/posts?include=user
    //返回结果,就是把user信息添加到结果里了
//    {
//    "data": [
//    {
//    "id": 1,
//    "user_id": 1,
//    "title": "测试个标题",
//    "content": "简单内容测试",
//    "created_at": "2016-10-20 08:57:09",
//    "user": {
//    "data": {
//    "id": 1,
//    "email": "xwiwi@tom.com",
//    "name": "elick",
//    "avatar": null,
//    "created_at": "2016-10-20 08:48:10",
//    "updated_at": "2016-10-20 08:48:10",
//    "deleted_at": null
//    }
//    }
//    }, ......
    public function includeUser(Post $post)
    {
        // 这句就是model实例,其中的user方法. 看下面post的model
        return $this->item($post->user, new UserTransformer());
    }

    // 带参数这个还不不对,如何用url传参还没弄明白
    public function includeComments(Post $post, ParamBag $params = null)
    {
        $limit = 10;
        if ($params) {
            $limit = (array) $params->get('limit');
            $limit = (int) current($limit);
        }

        $comments = $post->comments()->limit($limit)->get();
        $total = $post->comments()->count();

        return $this->collection($comments, new PostCommentTransformer())->setMeta(['total' => $total]);
    }
}

```

**Post**
```php

namespace ApiDemo\Models;

use Illuminate\Database\Eloquent\SoftDeletes;

class Post extends BaseModel
{
    use SoftDeletes;

    protected $casts = ['extra' => 'array'];

    public function user()
    {
        return $this->belongsTo('ApiDemo\Models\User');
    }

    public function comments()
    {
        return $this->hasMany('ApiDemo\Models\PostComment');
    }
}
```


[官方文档](http://fractal.thephpleague.com/transformers/)
[Github说明](https://github.com/dingo/api/wiki/Transformers)


