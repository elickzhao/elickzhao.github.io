title:  PHP 获取服务器详细信息代码
date: 2016-04-29 23:37:15
tags: [php,linux]
---

>说明一下为什么写这个.因为docker的link时需要取得环境变量里面的mysql容器的IP地址,所以想用php取得容器的环境变量.原本想用 `$_ENV[]` 发现没有内容 原来需要修改 **php.ini** 里面 `variables_order = "EGPCS"`
上述配置表示PHP 接受的外部变量来源及顺序，EGPCS 是Environment、Get、Post、Cookies 和Server 的缩写。如果variables_order 的配置中缺少E ，则PHP 无法接受环境变量，那么`$_ENV` 也就为空了。 后来想到用 php 执行 shell 命令 使用exec , system , shell_exec  但还是获取不到 好赖用 system(env) 才发现原来是我登录的用户为 www-data 所以根本获取不到额外的环境变量 所以这个想法只能作罢, 至于下面 是顺手总结的 一些全局变量的用法

<!--more-->


| 功能        | 代码   |  备注  |
| --------   | :-----:  | :----:  |
|获取系统类型及版本号：    | `php_uname()`     |(例：Windows NT COMPUTER 5.1 build 2600)
|只获取系统类型：          | `php_uname('s')`   |(或：PHP_OS，例：Windows NT)
|只获取系统版本号：        | `php_uname('r')`   |  无   
|获取PHP运行方式：      | `php_sapi_name()`      | (PHP run mode：apache2handler)
|获取前进程用户名：       | `Get_Current_User()` | 无
|获取PHP版本：         | `PHP_VERSION` | 无
|获取Zend版本：         | `Zend_Version()` | 无
|获取PHP安装路径：     | `DEFAULT_INCLUDE_PATH` | 无
|获取当前文件绝对路径：  |  __FILE__ | 无
|获取客户端IP：          |  `$_SERVER['REMOTE_ADDR']` | 无
|获取服务器解译引擎：    |  `$_SERVER['SERVER_SOFTWARE']`  | 无
|获取服务器CPU数量：     | `$_SERVER['PROCESSOR_IDENTIFIER']` | 无
|获取服务器系统目录：    |  `$_SERVER['SystemRoot']` | 无
|获取服务器域名：       | `$_SERVER['SERVER_NAME']  | (建议使用：$_SERVER["HTTP_HOST"])`
|获取用户域名：          |  `$_SERVER['USERDOMAIN']` | 无
|获取服务器语言：        |  `$_SERVER['HTTP_ACCEPT_LANGUAGE']` | 无
|获取服务器Web端口：     | `$_SERVER['SERVER_PORT']` | 无
| 获取服务器IP：      | `GetHostByName($_SERVER['SERVER_NAME'])` | 推荐
|获取Http请求中Host值：   | `$_SERVER["HTTP_HOST"]`  | 返回值为域名或IP
|接受请求的服务器IP：    | `$_SERVER["SERVER_ADDR"]` | (有时候获取不到，)



在PHP网站开发中，为了满足网站的需要，时常需要对PHP环境变量进行设置和应用，在虚拟主机环境下，有时我们更需要通过PHP环境变量操作函 数来对PHP环境变量值进行设置。为此我们有必要对PHP环境变量先有所熟悉。今天和大家分享PHP环境变量$_SERVER和PHP系统常量的部分详细 说明。

PHP提供了很多默认的系统变量，用于获得系统配置信息、网络请求相关信息等。这些默认的系统变量及其作用如表2-1所示。

|变量 | 作用
| ----- | :------:
|`$GLOBALS[]` |储存当前脚本中的所有全局变量，其KEY为变量名，VALUE为变量值
|`$_SERVER[]` | 当前WEB服务器变量数组
|`$_GET[]` | 存储以GET方法提交表单中的数据
|`$_POST[]` | 存储以POST方法提交表单中的数据
|`$_COOKIE[]` | 取得或设置用户浏览器Cookies中存储的变量数组
|`$_FILES[] `|存储上传文件提交到当前脚本的数据
|`$_ENV[]` | 存储当前WEB环境变量
|`$_REQUEST[]` |存储提交表单中所有请求数组，其中包括`$_GET、$_POST、$_COOKIE和$_SESSION`中的所有内容
|`$_SESSION[]` |存储当前脚本的会话变量数组

配置文件的不同，在不同环境下显示的内容可能会有所不同。

与系统变量一样，PHP也提供了一些默认的系统常量供使用。在程序中可以随时应用这些系统常量，但是我们不能任意更改这些常量的值。PHP中常用的一些默认系统常量及其作用如表2-2所示。

|常量 | 作用
| ------ | :------:
|__FILE__ |存储当前脚本的绝对路径及文件名称
|__LINE__ | 存储该常量所在的行号
|__FUNCTION__ | 存储该常量所在的函数名称
|__CLASS__ | 存储该常量所在的类的名称
|PHP_VERSION | 存储当前PHP的版本号
|PHP_OS | 存储当前服务器的操作系统

 `$_GET` 和`$_POST`主要针对FORM表单提交的数据，
 `$_COOKIE`和`$_SESSION`主要针对客户端游览器和服务器端会话数据。
 `$_FILES`主要针对文件上传时提交的数据，
 `$_REQUEST`主要针对提交表单中所有请求数组，包括`$_GET、$_POST`、
 `$_COOKIE`中的所有内容，你可以通过`print_r`函数分别输出`$_REQUEST`或者`$_COOKIE`等进行比较。

PHP环境变量$_SERVER简介

　　是一个包含服务器端相关信息的PHP全局环境变量，在PHP4.1.0之前的版本使用$HTTP_SERVER_VARS。

　　`$_SERVER['PHP_SELF']` 当前正在执行脚本的文件名，与 document root相关。在FORM表单中，如执行文件是本身，你可以在ACTION中使用`$_SERVER['PHP_SELF']`，好处是当执行文件名有变动时可以不去频繁替换ACTION中的文件名。

|变量|作用
|-----|:-----:
|`$_SERVER['SERVER_NAME']` | 当前运行的PHP程序所在服务器主机的名称。
|`$_SERVER['REQUEST_METHOD']`| 访问页面时的请求方法，即GET、HEAD、POST、PUT。
|`$_SERVER['DOCUMENT_ROOT']` | 当前运行的PHP程序所在的文档根目录。也就是PHP.INI文件中的定义。
|`$_SERVER['HTTP_REFERER']` | 链接到当前页面的前一页面的URL地址。在页面跳转功能中非常有用。
|`$_SERVER['REMOTE_ADDR']` | 正在浏览当前页面访问者的IP地址。
|`$_SERVER['REMOTE_HOST']`| 正在浏览当前页面用户的主机名。
|`$_SERVER['REMOTE_PORT']`| 正在游览的用户连接到服务器时所使用的端口。
|`$_SERVER['SCRIPT_FILENAME']`| 当前执行脚本的绝对路径名。
|`$_SERVER['SERVER_PORT']` | 服务器所使用的端口
|`$_SERVER['SCRIPT_NAME']`| 包含当前脚本的路径。这在页面需要指向自己时非常有用。
|`$_SERVER['REQUEST_URI']`| 访问此页面所需的URI。如“/index.html”。
|`$_SERVER['PHP_AUTH_USER']`| 应用在HTTP用户登录认证功能中，这个变量是用户输入的用户名。
|`$_SERVER['PHP_AUTH_PW']` | 应用在HTTP用户登录认证功能中，这个变量便是用户输入的密码。
|`$_SERVER['AUTH_TYPE']`| 应用在HTTP用户登录认证功能中，这个变量便是认证的类型。

　　注：上述提到的这些PHP全局环境变量，在php.ini中的register_globals设置为on时，这些变量在所有PHP程序脚本中都可用，也就是$_SERVER数组被分离了。当然为了安全考虑，还是不要将register_globals打开为好。



　　PHP环境变量$_SERVER的更多信息请参考PHP帮助手册，文章开头提到在虚拟主机环境下我们需要通过PHP环境变量操作函数来对PHP环境变量值进行设置，主要用到ini_set和ini_get，其实还有更多此类函数，比如PHP中的错误报告设置等，其实都涉及到PHP.INI中的相关内容，有机会下次分享。