---
title: EFK-从零搭建日志系统
categories:
  - ELK
tags:
  - 日志系统
  - ELK
date: 2017-10-19 18:57:32
---

## 分析
日志需要包含字段：
1. time
2. level
3. app_id
4. instance_id
日志从产生到消费，主要经历以下几个阶段：采集->传输->切分->检索。

## 区分实例 instance_id
通过设置filebeat

## 系统搭建
### Filebeat 安装
```
curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-5.0.2-x86_64.rpm
rpm -vi filebeat-5.0.2-x86_64.rpm

vim /etc/filebeat/filebeat.yml
修改 output.elasticsearch hosts 
添加模板
output.elasticsearch:
  hosts: ["localhost:9200"]
  template.name: "filebeat"
  template.path: "filebeat.template.json"
  template.overwrite: false
启动
/etc/init.d/filebeat start
service filebeat start
```

### elasticsearch 安装
```
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.6.3.rpm
sha1sum elasticsearch-5.6.3.rpm 
sudo rpm --install elasticsearch-5.6.3.rpm

### NOT starting on installation, please execute the following statements to configure elasticsearch service to start automatically using chkconfig
 sudo chkconfig --add elasticsearch
### You can start elasticsearch service by executing
 sudo service elasticsearch start

service elasticsearch start

```
启动异常：Starting elasticsearch: Elasticsearch requires at least Java 8 but your Java version from /usr/bin/java does not meet this requirement 原因是java8安装，java版本管理工具没有更改 
```
ll /usr/bin/java
/usr/bin/java -> /etc/alternatives/java

update-alternatives --config java

update-alternatives --install /usr/bin/java java /usr/local/java/jdk/jdk1.8.0_112/bin/java 300
update-alternatives --install /usr/bin/javac javac /usr/local/java/jdk/jdk1.8.0_112/bin/javac 300

```

验证是否成功 curl http://localhost:9200/


### 安装Kibana
```
wget https://artifacts.elastic.co/downloads/kibana/kibana-5.6.3-i686.rpm
sha1sum kibana-5.6.3-i686.rpm 
sudo rpm --install kibana-5.6.3-i686.rpm

service kibana start
service kibana stop

访问 http://localhost:5601
```

### 修改日志收集路径
vim /etc/elasticsearch/elasticsearch.yml
设置
path.data: /mydata/elk/elasticsearch/data
path.logs: /mydata/elk/elasticsearch/logs
如果重启会报错
detected index data in default.path.data [/var/lib/elasticsearch/nodes/0/indices] where there should not be any
需要把/var/lib/elasticsearch/下的文件夹移动到/mydata/elk/elasticsearch/下，*删除*/var/lib/elasticsearch/

### kibana使用
#### Lucene query syntax
* 简单的文本搜索，直接输入文本字符串。比如，如果你在搜索网站服务器日志，你可以输入 safari 来搜索各字段中的 safari 单词。
* 要搜索特定字段中的值，则在值前加上字段名。比如，你可以输入 status:200 来限制搜索结果都是在 status 字段里有 200 内容。
* 要搜索一个值的范围，你可以用范围查询语法，[START_VALUE TO END_VALUE]。比如，要查找 4xx 的状态码，你可以输入 status:[400 TO 499]。
* 要指定更复杂的搜索标准，你可以用布尔操作符 AND, OR, 和 NOT。比如，要查找 4xx 的状态码，还是 php 或 html 结尾的数据，你可以输入 status:[400 TO 499] AND (extension:php OR extension:html)。

## 检查健康状况
curl localhost:9200/_cat/health
## 查看索引
curl -XGET 'http://localhost:9200/_cat/indices?v'
## 删除索引
curl -XDELETE 'http://10.1.1.92:9200/filebeat-2017.10.*'

## 参考
https://mp.weixin.qq.com/s/onrBwQ0vyLJYWD_FRnNjEg
ELKstack 中文指南：https://kibana.logstash.es/content/kibana/v5/discover.html

