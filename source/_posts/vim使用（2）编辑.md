---
title: vim使用（2）编辑
categories:
  - Linux
tags:
  - vim
date: 2018-05-23 23:44:13
---

## 窗口
1. 创建窗口
```
水平分割窗口
:split             当前窗口一分为二，两个窗口显示相同内容。  
:10split           新窗口的高度10行
:split otherfile   新窗口中打开otherfile   
:new               功能和split一样  
:sp                split的缩写形式  
ctrl+w+s           分割窗口的快捷方式

垂直分割窗口
:vsplit 以上所有命令都适用于打开垂直分割窗口，只要在前面加v(vetical)
```

2. 关闭窗口
```
q  或 close   #关闭当前窗口
 only      #保留当前窗口，关闭其它所有窗口
 qall(qa)      #退出所有窗口
 wall      #保存所有窗口
```


3. 窗口切换
:ctrl+w+j/k，通过j/k可以上下切换，或者:ctrl+w加上下左右键，还可以通过快速双击ctrl+w依次切换窗口。

4. 窗口大小调整
```
纵向调整
:ctrl+w + 纵向扩大（行数增加）
:ctrl+w - 纵向缩小 （行数减少）
:res(ize) num  例如：:res 5，显示行数调整为5行
:res(ize)+num 把当前窗口高度增加num行
:res(ize)-num 把当前窗口高度减少num行
横向调整
:vertical res(ize) num 指定当前窗口为num列
:vertical res(ize)+num 把当前窗口增加num列
:vertical res(ize)-num 把当前窗口减少num列
```

5. 窗口重命名
:f file

6. 文件浏览
```
:Ex 开启目录浏览器，可以浏览当前目录下的所有文件，并可以选择
:Sex 水平分割当前窗口，并在一个窗口中开启目录浏览器
:ls 显示当前buffer情况
```

7. vi与shell切换
```
:shell 可以在不关闭vi的情况下切换到shell命令行
:exit 从shell回到vi    
```

## 页签

```
:tabnew [++opt选项] ［＋cmd］ 文件            建立对指定文件新的tabta
:tab <文件>  新页签打开指定文件
:tabc       关闭当前的tab
:tabo       关闭所有其他的tab
:tabs       查看所有打开的tab
:tabp      前一个
:tabn      后一个
标准模式下：
gt , gT 可以直接在tab之间切换。
ngt 切换到指定的窗口。
更多可以查看帮助 :help table ， help -p
```

## 编译运行
vim 环境下，在命令模式中输入下命令：
```
:!python %
```

## NERDTree命令
1. 窗口切换
    ctrl + w + h    光标 focus 左侧树形目录
    ctrl + w + l    光标 focus 右侧文件显示窗口
    ctrl + w + w    光标自动在左右侧窗口切换
    ctrl + w + r    移动当前窗口的布局位置
2. o 打开关闭文件或者目录，如果是文件的话，光标出现在打开的文件中 
go 效果同上，不过光标保持在文件目录里，类似预览文件内容的功能 
i和s可以水平分割或纵向分割窗口打开文件，前面加g类似go的功能
3. t 在标签页中打开
4. T 在后台标签页中打开

```
o       在已有窗口中打开文件、目录或书签，并跳到该窗口
go      在已有窗口 中打开文件、目录或书签，但不跳到该窗口
t       在新 Tab 中打开选中文件/书签，并跳到新 Tab
T       在新 Tab 中打开选中文件/书签，但不跳到新 Tab
i       split 一个新窗口打开选中文件，并跳到该窗口
gi      split 一个新窗口打开选中文件，但不跳到该窗口
s       vsplit 一个新窗口打开选中文件，并跳到该窗口
gs      vsplit 一个新 窗口打开选中文件，但不跳到该窗口
```

5. p 到上层目录
6. P 到根目录
7. K 到同目录第一个节点
8. J 到同目录最后一个节点
9. m 显示文件系统菜单（添加、删除、移动操作）
10. ? 帮助
11. q 关闭
