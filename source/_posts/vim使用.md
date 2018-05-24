---
title: vim使用
date: 2017-09-21 12:32:30
categories:
    - Linux
tags:
    - vim
---

## vim编辑粘贴代码格式化
vim粘贴代码会有代码代码错乱的问题

>在粘贴前先设置进入粘贴插入模式，即不会自动缩进和连续注释
set paste
然后再进入插入模式粘贴
在粘贴插入模式下代码是不会自动按格式缩进的，需要使用nopaste设置回来
set nopaste
也可以在.vimrc中设置切换的快捷键，比如设置F9，则可以在.vimrc中加入：
set pastetoggle=<F9>
这样直接在插入模式按F9就会在“-- 插入 --”模式和“-- 插入（粘贴） --”模式中切换

## 移动
```
^                       移到当前行的第一个非空字符
$                       移到当前行的最后一个字符
G                       移到文件的最后一行
gg                      移到文件的第一行

```

## 文本操作
```
:r file             读入文件 file 内容，并插在当前行后
:nr file            读入文件 file 内容，并插在第 n 行后
:r !pwd             插入当前路径

yy     复制一行，此命令前可跟数字，标识复制多行，如6yy，表示从当前行开始复制6行
yw     复制一个字
y$     复制到行末

]p     有缩进的粘贴，vim会自动调节代码的缩进

格式化全文： gg=G
```
## 替换与操作
```
:g/text1/s/text2/text3        查找包含 text1 的行，用 text3 替换 text2
:g/text/command               在所有包含 text 的行运行 command 所表示的命令
:v/text/command               在所有不包含 text 的行运行 command 所表示的命令
```

## 保存文本和退出
```
:w                    保存文件但不退出 vi
:w file               将修改保存在 file 中但不退出 vi
:wq 或 ZZ 或 :x        保存文件并退出 vi
:q!                   不保存文件，退出 vi
:e!                   放弃所有修改，从上次保存文件开始再编辑
```

## vi 中的选项
```
:set number           显示行数
:set nonumber         取消显示行数
```

## vi 中的 shell 转义命令
```
:!command             执行 shell 的 command 命令，如 :!ls
:!!                         执行前一个 shell 命令
:r!command            读取 command 命令的输入并插入，如 :r!ls 会先执行 ls，然后读入内容
:w!command            将当前已编辑文件作为 command 命令的标准输入并执行 command 命令，如 :w!grep all
:cd directory         将当前工作目录更改为 directory 所表示的目录
:sh                   将启动一个子 shell，使用 ^d(ctrl+d) 返回 vi
:so file              在 shell 程序 file 中读入和执行命令
```

## vim 分屏

