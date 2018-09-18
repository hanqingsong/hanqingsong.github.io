---
title: zookeeper安装
date: 2018-08-29 16:56:28
categories:
    - zookeeper
tags:
    - zookeeper
---
## 下载解压
首先确认已经配置好java环境

```
# java -version
java version "1.8.0_172"
Java(TM) SE Runtime Environment (build 1.8.0_172-b11)
Java HotSpot(TM) 64-Bit Server VM (build 25.172-b11, mixed mode)
```

下载安装 zookeeper
```
wget http://mirrors.shu.edu.cn/apache/zookeeper/zookeeper-3.4.13/zookeeper-3.4.13.tar.gz

tar -zxf zookeeper-3.4.13.tar.gz -C /usr/local/zookeeper/
cd /usr/local/zookeeper/zookeeper-3.4.13/
mkdir data
```

修改配置
```
# vim conf/zoo.cfg

dataDir=/usr/local/zookeeper/zookeeper-3.4.13/data
```

启动服务
```
# ./bin/zkServer.sh start
ZooKeeper JMX enabled by default
Using config: /usr/local/zookeeper/zookeeper-3.4.13/bin/../conf/zoo.cfg
Starting zookeeper ... STARTED
```

启动CLI
```
[root@VM_0_8_centos bin]# ./zkCli.sh
Connecting to localhost:2181
```
