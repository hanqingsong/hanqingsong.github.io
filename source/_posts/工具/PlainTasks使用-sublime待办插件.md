---
title: PlainTasks使用-sublime待办插件
date: 2017-09-21 16:54:51
categories:
    - 工具
tags:
    - sublime
    - todo
---

## PlainTasks
PlainTasks插件可以在我们的sublime编辑器里记录待办事，很方便，页面也很简单美观。

## 效果图

![](http://ww1.sinaimg.cn/mw690/99c92116ly1fpqg9a1l24j20p00ml0wh.jpg)


## 安装package control组件
没有安装过插件的需要先安装package control组件才能安装插件
Ctrl+ ` 调出console
```
import urllib.request,os; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); open(os.path.join(ipp, pf), 'wb').write(urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ','%20')).read())
```

## 创建todo文件
以下类型文件会被识别为todo文件

```
 TODO
 *.todo
 *.todolist
 *.taskpaper
 *.tasks
```

## 常用操作

1. 创建project：输入一行文字在后方加上英文冒号就可以被识别为project，project是可以被折叠的，比如在任何地方输入`Projects:`
2. 创建task：⌘+enter或者⌘+i, ⌘+shift+enter自动加上创建时间
3. 完成task：⌘+d，两次⌘+d取消
4. 打标签：在行尾添加@ ，*前边有空格*
5. 上下行互换位置：⌘+control+up/down
6. 归档:归档文件所有完成的任务⌘+shift+A,已完成的任务移动至文档结尾;
7. 分割线：--- ✄ ------，输入--，按tab键
8. 设置优先级：行尾加@critical、@high、@low、@today （严重、高、低、今天）
9. 时间标记：
    - 开始时间：输入s,两次tab键 @started(13-10-25 15:20)
    - 开关：状态切换输入tg,两次tab键 @toggle(13-10-25 15:20)
    - 创建：创建输入cr,两次tab键 @created(13-10-25 15:20)
    - 预期：输入d,两次tab键 @due()
        + @due(17.1.1 1:1) 指定时间
        + @due(+w) = @due( +7) 当前时间加1周
        + @due(1) 1下月的第一天 ，2下月第二天
        + @due(+1) = @due(+) 当前时间加1天
        + @due(+2:) = @due(+2.) 当前时间加2h
        + @due(+.2) = @due(+0.2) 当前时间加2m
        + @due(+2.2) 当前时间加2h2m
        + @due(+2 2.2) 当前时间加2d2h2m



