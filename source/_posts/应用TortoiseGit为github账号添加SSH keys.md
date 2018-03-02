title: 应用TortoiseGit为github账号添加SSH keys
date: 2016-04-19 18:35:30
tags: [Git,ssh]
---

**呵呵 我算是比较懒的 不过避免重复造轮子这种降低效率的事 还是直接引入别人写得详细的文章吧**

这个方法可以解决TortoiseGit的问题,但是无法解决hexo上传的问题.
因为hexo用到的密钥其实是ssh的,并不是TortoiseGit里面的.
所以解决了TortoiseGit也不代表hexo就可用了.

~~**看来我得回家看看了,怎么用一个密码实现了hexo和TortoiseGit两个同时上传**
目前来看必须使用两个才行,TortoiseGit使用的是putty,但是hexo必须用ssh才可以啊. 所以家里那个电脑怎么做到的.~~
上面这个问题我想起来了,putty那个生成密码的工具,还可以转换密码,把ssh转换成putty密码.这样就可以使用一个公钥了.这样hexo和TortoiseGit

简单说下步骤吧,ssh生成密钥,用putty转换密钥->保存私钥,用putty自动加载转换工具再次转换以供TortoiseGit提交github用.

[看最下面](https://www.zybuluo.com/mdeditor#319400)

[文章地址](http://jingyan.baidu.com/article/63f236280f7e750209ab3d60.html)

