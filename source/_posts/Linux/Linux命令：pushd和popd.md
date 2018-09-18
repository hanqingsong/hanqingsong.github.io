---
title: Linux命令：pushd和popd
categories:
  - Linux
tags:
  - 命令
date: 2018-05-04 13:36:45
---

## pushd和popd
pushd 常用于将目录加入到栈中，加入记录到目录栈顶部，并切换到该目录
popd 用于删除目录栈中的记录

## 使用
```
pushd /usr/local/xxxx && git pull && popd

```