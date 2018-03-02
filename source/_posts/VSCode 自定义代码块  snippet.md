---
title: VSCode 自定义代码块  snippet
date: 2017-05-29 22:33:53
tags: IDE
categories:
---

## 什么是 snippet

snippet[ˈsnɪpɪt]，或者说「code snippet」，指的是能够帮助输入重复代码模式串，比如循环或条件语句，的模板。通过 snippet ，我们仅仅输入一小段代码就可以生成预定义的模板代码，甚至可以通过内部跳转快速补全模板。

![](http://img.blog.csdn.net/20170112141959764?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbWFva2Vsb25nOTU=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


<!-- more -->

## 如何配置 snippet

操作流程

1. 进入 snippet 设置文件
    1. 这里提供了两种方法：
    2. 按「Alt」键切换菜单栏，通过文件 > 首选项 > 用户代码片段，选择进入目的语言的代码段设置文件；
通过快捷键「Ctrl + Shift + P」打开命令窗口（all command window），输入「snippet」，通过候选栏中的选项进入目的语言的代码段设置文件。
2. 填写 snippets

**例子**
```js
{
	/*
	 // Place your snippets for TypeScript here. Each snippet is defined under a snippet name and has a prefix, body and
	 // description. The prefix is what is used to trigger the snippet and the body will be expanded and inserted. Possible variables are:
	 // $1, $2 for tab stops, $0 for the final cursor position, and ${1:label}, ${2:another} for placeholders. Placeholders with the
	 // same ids are connected.
	 // Example:
	 "Print to console": {
		"prefix": "log",
		"body": [
			"console.log('$1');",
			"$2"
		],
		"description": "Log output to console"
	}
*/
    //这是个json格式的文件
	"Print to for": {
		"prefix": "forz",   //快捷输入
		"body": [           //格式代码构成
			"for(var  ${1:i} = 0; ${1:i} < ${2:len}; ${1:i}++ ){",
				"	$0",
			"}"
		],
		"description": "for循环快速编写		for(var i = 0; i < len; i++ ){			...		} 	"   //注释
	}
}
```

## snippet 由三部分组成：

- prefix：前缀，定义了 snippets 从 IntelliSense 中呼出的关键字;
- body： 主体，即模板的主体内容，其中每个字符串表示一行;
- description：说明，会在 IntelliSense 候选栏中出现。未定义的情况下直接显示对象名，上例中将会显示 Print to console。

其中 body 部分可以使用特殊结构来控制光标和要插入的文本。 支持的功能及其文法如下：

- Tabstops：制表符
>用「Tabstops」可以让编辑器的指针在 snippet 内跳转。使用 $1，$2 etc. 指定光标位置。
这些数字指定了Tabstops将被访问的顺序，特别地，$0表示最终光标位置。
相同序号的「Tabstops」被链接在一起，将会同步更新.
比如下列用于生成头文件封装的 snippet 被替换到编辑器上时，光标就将同时出现在所有$1位置。

- Placeholders：占位符
>「placeholder」是带有默认值的「Tabstops」，如${1：foo}。「placeholder」文本将被插入「Tabstops」位置，并在跳转时被全选，以方便修改。占位符还可以嵌套，例如${1:another ${2:placeholder}}。

- Variables：变量
>使用$name或${name:default}可以插入变量的值。 当未设置变量时，将插入其缺省值或空字符串。 当varibale未知（即，其名称未定义）时，将插入变量的名称，并将其转换为「placeholder」。 可以使用以下「Variable」：
>- TM_SELECTED_TEXT：当前选定的文本或空字符串
>- TM_CURRENT_LINE：当前行的内容
>- TM_CURRENT_WORD：光标下的单词的内容或空字符串
>- TM_LINE_INDEX：基于零索引的行号
>- TM_LINE_NUMBER：基于一索引的行号
>- TM_FILENAME：当前文档的文件名
>- TM_DIRECTORY：当前文档的目录
>- TM_FILEPATH：当前文档的完整文件路径
**注意，这些都是变量名，不是宏，在实际使用的时候还是要加上$符的。**


>拓展阅读：官网也给出了 snippet 的 EBNF 范式的正则文法，注意，使用\（反斜杠）转义\$, ,, }和\。
any ::= tabstop | placeholder | variable | text
tabstop ::= '$' int | '${' int '}'
placeholder ::= '${' int ':' any '}'
variable ::= '$' var | '${' var }' | '${' var ':' any '}'
var ::= [_a-zA-Z] [_a-zA-Z0-9]*
int ::= [0-9]+
text ::= .*


有用的建议

默认情况下 snippet 在 IntelliSense 中的显示优先级并不高，而且在 IntelliSense 中选择相应 snippet 需要按「enter」键，这对于手指短的人来说并不是什么很好的体验。所幸，VSCode 意识到了这一点，并为我们提供了改进的方式。

在 VSCode 的用户设置（「Ctrl+P」在输入框中写「user settings」后点选）中，检索代码段，然后根据提示修改，设置建议优先显示，并且可以通过「TAB」补全 snippet。

![](http://img.blog.csdn.net/20170112155152376?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbWFva2Vsb25nOTU=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

## 参考文档
[http://blog.csdn.net/maokelong95/article/details/54379046](http://blog.csdn.net/maokelong95/article/details/54379046)
