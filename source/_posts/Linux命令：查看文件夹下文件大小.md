---
title: Linux 命令：查看文件夹下文件大小
date: 2017-10-13 15:48:34
categories:
    - Linux
tags:
    - 命令
---
## 查看文件夹下文件大小
du [-abcDhHklmsSx] [-L <符号连接>][-X <文件>][--block-size][--exclude=<目录或文件>] [--max-depth=<目录层数>][--help][--version][目录或文件]

```
du -h --max-depth=1 /mydata/ 
```