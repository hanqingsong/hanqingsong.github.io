---
title: tmux使用
date: 2017-09-21 12:08:03
categories: 
    - 工具
tags: 
    - tmux
---

## tmux
tmux是linux中一种管理窗口的程序。tmux提供了一个窗体组随时存储和恢复的功能。

## tmux的基本概念
tmux是一个终端复用器(terminal multiplexer).
tmux有三个概念：会话(Session)，窗口(Window)，面板(Pane)。
当你输入tmux后, tmux实际做的事是首先创建一个会话(Session), 然后在这个会话中创建一个窗口, 你可以继续创建多个窗口(Window), 每个窗口初始只包含一个面板, 继续分屏后, 会出现多个面板(Pane) 你在其中看到的终端实际上都属于tmux的某个面板。

## tmux安装
```
[root@VM_1_49_centos ~]# yum install tmux
```

## 命令行

- tmux new[-session] -s name -d   新建会话(-d 是否在后台)
- tmux new -s name -n windowname  新建会话及窗口
- tmux at[tach] -t session    重新连接(-t 后接会话名称)
- tmux ls 显示保存的会话
- tmux kill-session -t session    删除会话

## 常用操作

所有快捷键执行方式：
```
 control+b 告诉Tmux我要用Tmux的快捷键。
```

- C-b ? 列出所有快捷键, 按q或Esc返回
- C-b d detach当前会话,可暂时返回Shell界面，输入tmux attach能够重新进入之前会话
- C-b s 选择并切换会话；在同时开启了多个会话时使用

## 快捷键
### sesson
- tmux new -s session_name
    creates a new tmux session named session_name
- tmux attach -t session_name
    attaches to an existing tmux session named session_name
- tmux switch -t session_name
    switches to an existing session named session_name
- tmux list-sessions
    lists existing tmux sessions
- tmux detach (prefix + d)
    detach the currently attached session
- C-b $ 改变会话的名字

```
# 创建一个新的session
$ tmux new -s <name-of-my-session>
# 在当前session中创建一个新的Session, 并保证之前session依然存在
# C-b : 然后输入下面命令
new -s <name-of-my-new-session>
# 进入名为test的session
$ tmux attach -t test
```

### window
- C-b c 创建一个新窗口
- C-b & 关闭当前窗口
- C-b w 列出所有的窗口选择
- C-b p 切换到上一个窗口
- C-b n 切换到下一个窗口
- C-b 窗口号 使用窗口号切换窗口(例如窗口号为1的, 则C-b 1)
- C-b , 重命名当前窗口，便于识别各个窗口
- C-b . 修改当前编号

### pane
- C-b % 横向分Terminal
- C-b " 纵向分Terminal
- C-b 方向键 则会在自由选择各面板
- C-b x 关闭当前pane
- C-b q 显示面板编号
- C-b o 选择当前窗口中下一个面板
- C-b 数字 选择指定窗口
- C-b z  暂时把一个窗体放到最大

## 开启批量执行（同步输入）

C-b后输入:set synchronize-panes ，输入:set sync [TAB]键可自动补齐

## 设置鼠标
C-b : 进入命令行

```
setw mode-mouse on #单个窗口启用鼠标滚轮来卷动窗口内容

setw -g mode-mouse on #所有窗口启用鼠标滚轮来卷动窗口内容
```

或者在配置文件里设置~/.tmux.conf：
```
setw -g mouse-resize-pane on #开启用鼠标拖动调节pane的大小
setw -g mouse-select-pane on #开启用鼠标点击pane来激活该pane
setw -g mouse-select-window on #开启用鼠标点击来切换活动window 状态栏的窗口名称
setw -g mode-mouse on #开启所有窗口启用鼠标滚轮来卷动窗口内容

```

新版本设置
```
set-option -g mouse on
# 将window的起始设置为1
 set -g base-index 1
# 将pane的起始下标设为1
 set -g pane-base-index 1
```

配置之后进入命令行，输入 tmux source  ~/.tmux.conf，使配置生效。

## 进入vi copy模式
```
#vi
setw -g mode-keys vi
bind -t vi-copy v begin-selection
bind -t vi-copy y copy-selection
```
或者使用鼠标选中，按y复制

复制到系统剪贴板中
Mac 下如果用 iterm2 可以在 preference 下选择 Applications in terminal may access clipboard。



## 参考
http://cenalulu.github.io/linux/tmux/
http://guoqiao.me/post/2016/tmux