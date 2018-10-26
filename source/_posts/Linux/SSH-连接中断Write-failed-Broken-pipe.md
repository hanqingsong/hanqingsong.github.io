---
title: 'SSH 连接中断Write failed: Broken pipe'
date: 2017-09-21 13:47:37
categories:
    - 问题记录
tags:
    - SSH
    - 问题记录
---

ssh连接服务器后,一段时间不操作，再次操作时会出现Write failed: Broken pipe。
解决方法：
1、客户端设置：~/.ssh/ 文件夹中添加 config 文件，并添加下面的配置：ServerAliveInterval 60
2、在服务器的 /etc/ssh/sshd_config 中添加如下的配置：ClientAliveInterval 60
3、在登录命令添加参数：ssh -o ServerAliveInterval=60 user@sshserver

参考：
http://blog.csdn.net/bbirdsky/article/details/21703555