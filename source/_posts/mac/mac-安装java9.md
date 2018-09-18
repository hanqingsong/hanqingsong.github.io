---
title: mac 安装java9
date: 2017-09-27 09:51:38
categories:
    - java
tags:
    - java
---

## 官网下载
[jdk9官网地址](http://www.oracle.com/technetwork/java/javase/downloads/jdk9-downloads-3848520.html)

## 修改配置
mac环境修改配置文件
vim .bash_profile

```
export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk-9.jdk/Contents/Home/
export PATH=$PATH:$JAVA_HOME/bin
```

使配置文件生效 
source .bash_profile


