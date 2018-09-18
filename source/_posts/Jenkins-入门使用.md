---
title: Jenkins 入门使用
date: 2017-09-26 15:19:13
categories:
    - 工具
tags:
    - jenkins
---

## 下载
官网下载 jenkins.war
或者 brew install jenkins

## 启动和停止
启动
```
设置开机自启动：sudo launchctl load -w /Library/LaunchDaemons/org.jenkins-ci.plist
取消开机自启动：sudo launchctl unload -w /Library/LaunchDaemons/org.jenkins-ci.plist
手动启动：Java -jar jenkins.war
后台启动(默认端口)：nohup java -jar jenkins.war &
后台启动(指定端口)：nohup java -jar jenkins.war -httpPort=88 &
后台启动(HTTPS)：nohup java -jar jenkins.war -httpsPort=88 &

sudo launchctl load /Library/LaunchDaemons/org.jenkins-ci.plist
```

停止
```
sudo launchctl unload /Library/LaunchDaemons/org.jenkins-ci.plist
```

## 设置为中文
1. 安装Locale Plugin， 重启生效。
2. 配置【Manage Jenkins】>【Configure System】> 【Locale】
3. 语言填zh_CN，勾选强制设置语言

## 运行远程脚本
1. 安装Jenkins SSH plugin插件
2. 在系统配置里配置Publish over SSH
复制jenkins所在服务器密钥
配置远程ssh server
3. 增加构建步骤send files or ececute commands over SSH
```
SSH  Server Name：选个一个你在系统设置里配置的名字
Transfer Set Source files：需要上传的文件（注意：相对于工作区的路径。看后面的配置可以填写多个，默认用,分隔）
Remove prefix：移除目录（只能指定Transfer Set Source files中的目录，这里移除了target目录表示只将FinServer.war传到目标服务器，否则会在目标服务器创建target目录）
Remote directory：远程目录（根据你的需求填写，这里没有填写默认会继承系统配置，即/mnt）
Exec command：把你要执行的命令写在里面(这里的命令是在目标服务器上执行的)
```
只需要配置Exec command就可以
这种远程执行脚本的方式，属于非交互式Shell，不会触发诸如~/.bash_profile之类文件的载入,需要 export设置环境变量或者source <profile>



