---
title: Linux：ssh登录后闲置时间过长而断开连接
categories:
  - Linux
tags:
  - 命令
date: 2018-05-04 14:19:47
---

## ssh登录后闲置时间过长而断开连接

终端ssh登录远程服务器长时间不操作服务器就会自动断开连接
解决此问题的方法：

方法一：

1. \#vi /etc/ssh/sshd_config配置文件，修改为ClientAliveInterval 60，每分钟发送一次检测客户端是否连接 ，ClientAliveCountMax 3 服务器发出请求后客户端没有响应的次数达到一定值, 就自动断开
2. 修改完成并保存后，执行命令：#service sshd reload使配置立即生效！

方法二：

修改/etc/profile配置文件：#vi /etc/profile 增加：TMOUT=1800 这样30分钟没操作才自动LOGOUT
