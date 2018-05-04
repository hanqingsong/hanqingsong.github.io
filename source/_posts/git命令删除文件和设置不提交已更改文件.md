---
title: git命令设置不提交更改文件
categories:
  - 工具
tags:
  - git
date: 2018-04-21 09:32:54
---


## git rm 
需要删除暂存区或分支上的文件, 同时工作区也不需要这个文件了
```
git rm file_path 
```

## git rm --cache
需要删除暂存区或分支上的文件, 但本地又需要使用, 只是不希望这个文件被版本控制,
```
git rm --cached file_path
```
删除之后加入.gitignore
ps：.gitignore 只会对未加入版本控制的文件有效，如果已经加入过，需要从版本控制中移出

## git update-index
已经将这个文件提交到git库中,希望之后修改不再提交。

```
执行命令将file_path加入不提交队列
git update-index --assume-unchanged file_path

执行命令将xxx/xxx取消加入不提交队列
git update-index --no-assume-unchanged file_path
```