---
title: 服务器相关技术
date: 2016-11-20 22:32:46
tags:
categories: 服务器相关技术
---
yum是一个用于管理rpm包的后台程序，用python写成，可以非常方便的解决rpm的依赖关系。在建立好yum服务器后，yum客户端可以通过 http、ftp方式获得软件包，并使用方便的命令直接管理、更新所有的rpm包，甚至包括kernel的更新。它也可以理解为红旗环境下的apt管理工具。

   以前写过一份[原]使用yum更新红旗Linux ，但其中提到的命令不是很完整，现再整理一下。

一、列举包文件

列出资源库中所有可以安装或更新的rpm包

# yum list

列出资源库中特定的可以安装或更新以及已经安装的rpm包

# yum list perl           //列出名为perl  的包

# yum list perl*         //列出perl 开头的包

列出资源库中所有可以更新的rpm包

# yum list updates

列出已经安装的所有的rpm包

# yum list installed

列出已经安装的但是不包含在资源库中的rpm包

# yum list extras

注:extras是repos.d中定义的资源列表名称

二、列举资源信息

列出资源库中所有可以安装或更新的rpm包的信息

# yum info

列出资源库中特定的可以安装或更新以及已经安装的rpm包的信息

# yum info perl           //列出perl 包信息

# yum info perl*         //列出perl 开头的所有包的信息

列出资源库中所有可以更新的rpm包的信息

# yum info updates

列出已经安装的所有的rpm包的信息

# yum info installed

列出已经安装的但是不包含在资源库中的rpm包的信息

# yum info extras

三、搜索

搜索匹配特定字符的rpm包

# yum search perl            //在包名称、包描述等中搜索

搜索有包含特定文件名的rpm包

# yum provides realplay

四、管理包

安装rpm包

# yum install perl     //安装perl 包

# yum install perl*     //安装perl 开头的包

删除rpm包,包括与该包有倚赖性的包

# yum remove perl*            //会删除perl-* 所有包

五、更新

检查可更新的rpm包

# yum check-update

更新所有的rpm包

# yum update

更新指定的rpm包,如更新kernel和kernel source

# yum update kernel kernel-source

大规模的版本升级,与yum update不同的是,连旧的淘汰的包也升级

# yum upgrade

六、清空缓存

清除暂存中rpm包文件

# yum clean packages

清除暂存中rpm头文件

# yum clearn headers

清除暂存中旧的rpm头文件

# yum clean oldheaders

清除暂存中旧的rpm头文件和包文件

# yum clearn

或

# yum clearn all

七、其他

安装Livna.org rpms GPG key

# rpm --import http://rpm.livna.org/RPM-LIVNA-GPG-KEY

检查GPG Key

# rpm -qa gpg-pubkey*

显示Key信息

# rpm -qi gpg-pubkey-a109b1ec-3f6e28d5

删除Key

# rpm -e gpg-pubkey-a109b1ec-3f6e28d5
