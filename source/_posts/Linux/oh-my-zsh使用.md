---
title: oh-my-zsh使用
date: 2017-09-22 15:15:46
categories:
    - 工具
tags:
    - Linux
    - zsh
    - mac
---

## shell
查看当前的shell
>cat /etc/shells

```
# List of acceptable shells for chpass(1).
# Ftpd will not allow users to connect who are not using
# one of these shells.

/bin/bash
/bin/csh
/bin/ksh
/bin/sh
/bin/tcsh

```

## zsh 安装

```
CentOS 安装：sudo yum install -y zsh
mac 安装：brew install zsh
```

zsh设置成系统默认shell，以代替bash
> chsh -s /bin/zsh

## oh-my-zsh

oh-my-zsh 是 zsh 的一个配置工具。
安装
>wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | sh

## 安装插件

>vim .zshrc

修改 plugins=(git) 为 plugins=(git autojump)
使配置生效 source ~/.zshrc

## 修改主题

.zshrc 中修改ZSH_THEME=''

## 打开sublime配置

>alias st='open -a "Sublime Text"'