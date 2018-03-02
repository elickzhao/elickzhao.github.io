## 使用

在用IDE开发的时候,验证php代码的错误和标准格式化.

这是遇到了几个插件.在用VSCode的时候安装了 `php-cs-fixer` 
` composer global require friendsofphp/php-cs-fixer`
这个配置很简单只要用 `composer` 安装  `php-cs-fixer`  然后配置 php.exe 的位置就可以使用了 


在使用 `sublime` 的时候 安装了 `phpcs` 和 `phpmd `
安装也是用 `composer` 
`composer global require "squizlabs/php_codesniffer=*"`

这个配置(php code sniffer)就繁琐点了  配置如下


<!-- more -->
```
{
    // Plugin settings

    // Turn the debug output on/off
    "show_debug": false,

    // Which file types (file extensions), do you want the plugin to
    // execute for
    "extensions_to_execute": ["php"],

    // Do we need to blacklist any sub extensions from extensions_to_execute
    // An example would be ["twig.php"]
    "extensions_to_blacklist": [],

    // Execute the sniffer on file save
    "phpcs_execute_on_save": true,

    // Show the error list after save.
    "phpcs_show_errors_on_save": true,

    // Show the errors in the gutter
    "phpcs_show_gutter_marks": true,

    // Show outline for errors
    "phpcs_outline_for_errors": true,

    // Show the errors in the status bar
    "phpcs_show_errors_in_status": true,

    // Show the errors in the quick panel so you can then goto line
    "phpcs_show_quick_panel": true,

    // The path to the php executable.
    // Needed for windows, or anyone who doesn't/can't make phars
    // executable. Avoid setting this if at all possible
    "phpcs_php_prefix_path": "F:\\phpStudy\\php\\php-7.1.12-nts\\php.exe",

    // Options include:
    // - Sniffer
    // - Fixer
    // - MessDetector
    // - CodeBeautifier
    //
    // This will prepend the application with the path to php
    // Needed for windows, or anyone who doesn't/can't make phars
    // executable. Avoid setting this if at all possible
    "phpcs_commands_to_php_prefix": [],

    // What color to stylise the icon
    // https://www.sublimetext.com/docs/3/api_reference.html#sublime.View
    // add_regions
    "phpcs_icon_scope_color": "comment",


    // PHP_CodeSniffer settings

    // Do you want to run the phpcs checker?
    "phpcs_sniffer_run": true,

    // Execute the sniffer on file save
    "phpcs_command_on_save": true,

    // It seems python/sublime cannot always find the phpcs application
    // If empty, then use PATH version of phpcs, else use the set value
    "phpcs_executable_path": "C:\\Users\\elick\\AppData\\Roaming\\Composer\\vendor\\bin\\phpcs.bat",

    // Additional arguments you can specify into the application
    //
    // Example:
    // {
    //     "--standard": "PEAR",
    //     "-n"
    // }
    "phpcs_additional_args": {
        "--standard": "PSR2",
        "-n": ""
    },



    // PHP-CS-Fixer settings

    // Fix the issues on save
    "php_cs_fixer_on_save": false,

    // Show the quick panel
    "php_cs_fixer_show_quick_panel": false,

    // Path to where you have the php-cs-fixer installed
    "php_cs_fixer_executable_path": "C:\\Users\\elick\\AppData\\Roaming\\Composer\\vendor\\bin\\php-cs-fixer.bat",

    // Additional arguments you can specify into the application
    "php_cs_fixer_additional_args": {

    },



    // phpcbf settings

    // Fix the issues on save
    "phpcbf_on_save": false,

    // Show the quick panel
    "phpcbf_show_quick_panel": false,

    // Path to where you have the phpcbf installed
    "phpcbf_executable_path": ""phpcbf_executable_path": "C:\\Users\\elick\\AppData\\Roaming\\Composer\\vendor\\bin\phpcbf.bat",

    // Additional arguments you can specify into the application
    //
    // Example:
    // {
    //     "--level": "all"
    // }
    "phpcbf_additional_args": {
        "--standard": "PSR2",
        "-n": ""
    },



    // PHP Linter settings

    // Are we going to run php -l over the file?
    "phpcs_linter_run": true,

    // Execute the linter on file save
    "phpcs_linter_command_on_save": true,

    // It seems python/sublime cannot always find the php application
    // If empty, then use PATH version of php, else use the set value
    "phpcs_php_path": "F:\\phpStudy\\php\\php-7.1.12-nts\\php.exe",

    // What is the regex for the linter? Has to provide a named match for 'message' and 'line'
    "phpcs_linter_regex": "(?P<message>.*) on line (?P<line>\\d+)",



    // PHP Mess Detector settings

    // Execute phpmd
    "phpmd_run": false,

    // Execute the phpmd on file save
    "phpmd_command_on_save": true,

    // It seems python/sublime cannot always find the phpmd application
    // If empty, then use PATH version of phpmd, else use the set value
    "phpmd_executable_path": "C:\\Users\\elick\\AppData\\Roaming\\Composer\\vendor\\bin\\phpmd.bat",

    // Additional arguments you can specify into the application
    //
    // Example:
    // {
    //     "codesize,unusedcode"
    // }
    "phpmd_additional_args": {
        "codesize,unusedcode,naming": ""
    },


    // PHP Scheck settings

    // Execute scheck
    "scheck_run": false,

    // Execute the scheck on file save
    "scheck_command_on_save": false,

    // It seems python/sublime cannot always find the scheck application
    // If empty, then use PATH version of scheck, else use the set value
    "scheck_executable_path": "",

    // Additional arguments you can specify into the application
    //
    //Example:
    //{
    //  "-php_stdlib" : "/path/to/pfff",
    //  "-strict" : ""
    //}
    "scheck_additional_args": {
        "-strict" : ""
    }
}

```

## 说明

说一下 `phpcs` `php-cs-fixer` `phpmd` 都是干什么用的

**phpcs :**该包的作用是用指定的代码规范（默认使用PEAR规范，可指定使用PSR1，PSR2或自己制定的规范）来检查代码是否符合规范

**phpcbf :** phpcs自带了PHP Code Beautifier（phpcbf）用来修复不规范的代码 等同于 `php-cs-fixer`

**php-cs-fixer :** 修复不规范代码 作用等同于 `phpcbf` 只不过规则好像稍有区别

**phpmd :**  提示潜在的BUG, 有待改进的代码（比如过短变量名长度等）, 过于复杂的表达式, 定义但未使用的变量、方法、属性）, 使用未定义的变量

**sublime-phpcs :** sublime-phpcs 插件中各种功能（错误提示，代码格式化) 提供了把这些功能包在sublime中使用的界面。

## 参考文档 
[PHP代码规范与质量检查工具PHPCS,PHPMD的安装与配置](http://blog.csdn.net/cyaspnet/article/details/51773331)
[sublime安装PHPcs（PHPcodesniffer）代码规范提示插件](http://blog.csdn.net/he426100/article/details/76573038)
[MacOS Sublime Text 3 安装使用 sublime-phpcs 插件指南](http://www.uedbox.com/macos-install-sublime-phpcs/)