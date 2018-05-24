---
title: git使用记录
categories:
  - 工具
tags:
  - git
date: 2017-11-15 11:27:06
---

## 流程
```
  工作区 --->  暂存区 ---->  （历史）本地分支  --->   远程分支  
        add         commit                  push
```

## 工作区和暂存区
``` 
    工作区：在电脑里能看到的目录
    版本库：工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库。
    版本库里最重要的就是称为stage（或者叫index）的暂存区，(add操作)，
    还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD，（commit操作）。
```

## 分支创建与切换
```
git checkout -b dev #git checkout命令加上-b参数表示创建并切换
相当于
git branch dev #创建分支
git checkout dev #切换分支

git branch #查看分支
```

## 删除分支
```
git branch -d dev
```

## 合并分支
fast-forward 快速合并不产生合并节点，看不出来曾经做过合并
no-ff 合并生成合并节点

```
git merge dev #git merge命令用于合并指定分支到当前分支

git merge --no-ff -m "merge with no-ff" dev #合并生成合并节点
```

## 查看日志
```
git log #查看日志
git log --graph --pretty=oneline --abbrev-commit #图形查看日志
```

## 查看具体某次提交
```
git show 3ad960849d6f10c92356ecdf952d5e4bfd0df1cd
```

## 版本回退和恢复 
```   
    回退工作区修改文件
    git checkout -- <file> 

    回退暂存区修改文件（add操作）
    git reset HEAD <file>
    git checkout -- <file> 

    回退本地分支修改文件 （add commit操作）
    git reset --hard HEAD^ #git reset HEAD~2

    恢复git reflog 
    git reflog 
    git reset --hard 0a1f420
```

## 暂时储藏修改
当手头工作没有完成时，先把工作现场git stash一下，然后去修复bug，修复后，再git stash pop，回到工作现场
```
git stash  #储藏代码
git stash list #查看列表
git stash pop #回到工作现场
```

## Reset、Checkout、Revert 的选择

```
reset 将一个分支的末端指向另一个提交。
--soft – 缓存区和工作目录都不会被改变
--mixed – 默认选项。缓存区和你指定的提交同步，但工作目录不受影响
--hard – 缓存区和工作目录都同步到你指定的提交

checkout 除了分支之外，你还可以传入提交的引用来 checkout 到任意的提交。
git checkout HEAD~2

Revert 撤销一个提交的同时会创建一个新的提交,相比 git reset，它不会改变现在的提交历史。因此，git revert 可以用在公共分支上，git reset 应该用在私有分支上。你也可以把 git revert 当作撤销已经提交的更改，而 git reset HEAD 用来撤销没有提交的更改。

```


## git修改远程仓库地址方法有三种：
查看远程地址git remote -v

1. 修改命令 

```
git remote set-url origin [url]
```

2. 先删后加

```
git remote rm origin
git remote add origin [url]
```

3. 直接修改config文件

## git上新建项目之后

```
…or create a new repository on the command line
    echo "# hello" >> README.md
    git init
    git add README.md
    git commit -m "first commit"
    git remote add origin git@github.com:hanqingsong/hello.git
    git push -u origin master
    
…or push an existing repository from the command line
    git remote add origin git@github.com:hanqingsong/hello.git
    git push -u origin master
```

## 参考
https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000
https://github.com/geeeeeeeeek/git-recipes/wiki/5.2-%E4%BB%A3%E7%A0%81%E5%9B%9E%E6%BB%9A%EF%BC%9AReset%E3%80%81Checkout%E3%80%81Revert-%E7%9A%84%E9%80%89%E6%8B%A9
https://github.com/geeeeeeeeek/git-recipes/wiki