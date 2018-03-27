---
title: Linux命令常用记录
categories:
  - Linux
tags:
  - 命令
date: 2017-10-25 14:40:06
---

1. 创建多级文件夹 mkdir -p test1/test2/test3
2. 修改权限包含子目录 chown -R 
3. 创建软连接 ln [参数][源文件或目录][目标文件或目录] ln -s xx.log xxf.log
4. 递归拷贝这些及其子文件夹下的文件 cp -ri
5. 查看文件夹下文件大小 du -h --max-depth=1 /mydata/ 
6. 清屏 clear
7. 查看shell cat /etc/shells
8. 查看使用的shell echo $SHELL
9. 查看本地端口 netstat -ntlp
10. 