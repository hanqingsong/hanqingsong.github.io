---
title: docker编写Dockerfile
date: 2018-05-07 22:42:43
categories:
    - docker
tags:
    - docker
---

>MongoDB 是一个基于分布式文件存储的数据库。由C++语言编写。旨在为WEB应用提供可扩展的高性能数据存储解决方案。MongoDB 是一个介于关系数据库和非关系数据库之间的产品，是非关系数据库当中功能最丰富，最像关系数据库的数据库。特点是高性能、易部署、易使用，存储数据非常方便。

>Redis是一个开源的使用ANSI C语言编写、支持网络、可基于内存亦可持久化的日志型、Key-Value数据库，并提供多种语言的API。

## 安装mongodb
1. 直接使用mongo镜像
```
docker pull mongo
```

2. yum安装
vim /etc/yum.repos.d/mongodb-org-3.6.repo
```
[mongodb-org-3.6][mongodb-org-3.6]
 namename==MongoDB RepositoryMongoDB Repository
 baseurlbaseurl==https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.6/x86_64/https://repo.mongodb.org/yum/redha 
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.6.asc
```

```
yum install -y mongodb-org
 mongod # 启动
```

## docker run运行容器来创建MongoDB和Redis应用

## 参考资料
http://researchlab.github.io/2017/03/08/docker-run-mongodb-redis/