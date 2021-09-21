---
title: sublime text 插件 PlainTasks 任务计划
tags: IDE
abbrlink: 60372
date: 2017-12-27 22:50:01
categories:
---

还没正式开始使用 所以先记录几篇文章

哦 对了 因为字体原因 会造成图标不显示的问题 下面有具体解决方法 `Sublime Text 3的PlainTasks插件` 这篇文章.

这里简单说下,一 是改字体,改系统字体或者插件字体. 二 是把option后面能看到的图标 放到前面使用

说一下问题 自定义设置 `open_tasks_bullet` 和 `done_tasks_bullet` 如果这两个改了,就会出现 进度条无法显示的问题, 因为每次完成一个任务会直接把任务当作取消来算了. 
但是问题在于有时系统不会显示默认的字体.
这里就必须改写 `"font_face": "Microsoft YaHei",` 雅黑还算好看点,所以现在暂且用这个.并用默认图标


已经用了sublime的同步设置,下面保存这个其实没什么太多意义,不过还是保存一份吧
**自己的配置**

```js
{
  "open_tasks_bullet": "☐", // options: - | ❍ | ❑ | ■ | □ | ☐ | ▪ | ▫ | – | — | ≡ | → | › | [ ]
  "done_tasks_bullet": "✔", // options: + | ✓ | ✔ | √ | [x]
  "cancelled_tasks_bullet": "✘", // options: x | ✘ | [-]
  "before_tasks_bullet_margin": 1,
  "date_format": "(%y-%m-%d %H:%M)",
  "done_tag": true, // related to @cancelled as well
  "project_tag": true, // if true - postfix archived task with project tag, if false - prefix
  "archive_name": "Archive:", // make sure it is the unique project name within your todo files
  "new_on_top": true, // how to sort archived tasks
  "show_remain_due": false, // in Sublime 3, show remain or overdue time under due tags
  //按理说这是这进度条设计啊
  "stats_format":"$n/$a done ($percent%) $progress Last task @done $last",
  //"stats_format":"□$o [√]$d  ≡$a",
  "bar_full": "■",
  "bar_empty": "□", // empty cell for progress-bar in status-bar, for more details see Custom Statistics in README
  "replace_stats_chars": [[" ■", " [="], ["■", "="], ["▫", " ] "], ["▫", " "]],
  
  "color_scheme": "Packages/PlainTasks/tasks.hidden-tmTheme",
    // other bundled schemes:
    //   Packages/PlainTasks/tasks-dark.hidden-tmTheme
    //   Packages/PlainTasks/tasks-eighties-colored.hidden-tmTheme
    //   Packages/PlainTasks/tasks-eighties-dark.hidden-tmTheme
    //   Packages/PlainTasks/tasks-gray.hidden-tmTheme
    //   Packages/PlainTasks/tasks-monokai.hidden-tmTheme
    //   Packages/PlainTasks/tasks-solarized-dark.hidden-tmTheme
    //   Packages/PlainTasks/tasks-solarized-light.hidden-tmTheme
  // "font_size": 11,
  // "font_face": "Consolas",
  "draw_indent_guides": false,
  "line_numbers": false,
  "gutter": true,
  "margin": 2,
  "tab_size": 2,
  "translate_tabs_to_spaces": true,
  "use_tab_stops": false,
  "match_brackets": false,
  "fold_buttons": true,
  "fade_fold_buttons": false,
  "extensions":
  [
    "TODO",
    "todo",
    "todolist",
    "taskpaper",
    "tasks"
  ],
  "font_face": "Microsoft YaHei",
}

```
## 参考文档
[SublimeText 插件 - PlainTasks使用方法](http://blog.csdn.net/etimechen/article/details/50730150)
[Sublime Text 3的PlainTasks插件](https://zhuanlan.zhihu.com/p/20832754)
[SublimeText3 插件PlainTasks（Todo-list）的使用方法](http://blog.csdn.net/ljw_josie/article/details/73122997)
[SublimeText 插件 - PlainTasks使用方法](http://blog.csdn.net/yuenushan/article/details/50041283)