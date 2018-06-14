---
title: EFK-日志报警
date: 2018-06-04 17:43:41
categories:
  - ELK
tags:
  - 日志系统
  - ELK
---

## Elastalert 安装
下载 elastalert 源码
```
git clone https://github.com/Yelp/elastalert.git
cd elastalert
python setup.py install
pip install -r requirements.txt

```

安装完后，会在 /usr/local/bin/ 下生成4个elastalert命令
```
/usr/local/bin/elastalert 
/usr/local/bin/elastalert-create-index  
/usr/local/bin/elastalert-rule-from-kibana  
/usr/local/bin/elastalert-test-rule
```

elastalert-create-index 这个命令会在elasticsearch创建索引，这不是必须的步骤，但是强烈建议创建。因为对于，审计，测试很有用，并且重启elastalert不影响计数和发送alert,默认情况下，创建的索引叫 elastalert_status

## Elastalert 配置

## 配置文件config.yaml
/mydata/elk/elastalert/config.yaml

```
# 规则文件夹
rules_folder: my_rules

# 查询Elasticsearch时间间隔
run_every:
  minutes: 1

# ElastAlert缓存结果时间
buffer_time:
  minutes: 15

# Elasticsearch hostname
es_host: 127.0.0.1
es_port: 9200

# elasticsearch中的索引名
writeback_index: elastalert_status

# 发送失败重试时间间隔
alert_time_limit:
  days: 2

# 邮箱发送配置
smtp_host: smtp.exmail.qq.com
smtp_port: 465
smtp_ssl: true
smtp_auth_file: /mydata/elk/elastalert/email_auth.yaml
from_addr: xxx@xxx.cn
```

## 规则 example_rules
```
# 规则名，需要唯一
name: pay error frequency rule

# 数据验证方式
type: frequency

# 从某类索引里读取数据
index: filebeat-*

# 在时间间隔内匹配多少次触发报警
num_events: 1
# 时间间隔
timeframe:
  minutes: 1

# ES请求的过滤条件
filter:
- query:
    query_string:
        query: 'source:"/mydata/release/fs-pay/logs/error.log" AND  message:ERROR'
# 报警渠道
alert:
- "email"
email:
- "xxxx@xxx.cn"
```

自定义邮件内容可以配置 alert_text和alert_text_args

控制报警风暴，减少重复告警的频率
```   
# 用来区分报警，跟 realert 配合使用，在这里意味着，
# 5 分钟内如果有重复报警，那么当 name 不同时，会当做不同的报警处理，可以是数组
query_key:
 - name

# 5 分钟内相同的报警不会重复发送
realert:
 minutes: 5

# 指数级扩大 realert 时间，中间如果有报警，
# 则按照 5 -> 10 -> 20 -> 40 -> 60 不断增大报警时间到制定的最大时间，
# 如果之后报警减少，则会慢慢恢复原始 realert 时间
exponential_realert:
 hours: 1
```

### ruletype
```
any：只要有匹配就报警；

blacklist：compare_key字段的内容匹配上 blacklist数组里任意内容；

whitelist：compare_key字段的内容一个都没能匹配上whitelist数组里内容；

change：在相同query_key条件下，compare_key字段的内容，在 timeframe范围内 发送变化；

frequency：在相同 query_key条件下，timeframe 范围内有num_events个被过滤出 来的异常；

spike：在相同query_key条件下，前后两个timeframe范围内数据量相差比例超过spike_height。其中可以通过spike_type设置具体涨跌方向是- up,down,both 。还可以通过threshold_ref设置要求上一个周期数据量的下限，threshold_cur设置要求当前周期数据量的下限，如果数据量不到下限，也不触发；

flatline：timeframe 范围内，数据量小于threshold 阈值；

new_term：fields字段新出现之前terms_window_size(默认30天)范围内最多的terms_size (默认50)个结果以外的数据；

cardinality：在相同 query_key条件下，timeframe范围内cardinality_field的值超过 max_cardinality 或者低于min_cardinality
```

## 启动
启动elastalert服务，监听elasticsearch
```
python -m elastalert.elastalert --verbose --rule example_rules/example_test.yaml

后台运行
nohup python -m elastalert.elastalert --verbose --rule example_rules/example_test.yaml >/dev/null 2>&1 &
```

## 资料
https://elastalert.readthedocs.io/en/latest/
https://www.ctolib.com/docs/sfile/ELKstack-guide-cn/elasticsearch/other/elastalert.html
https://blog.xizhibei.me/2017/11/19/alerting-with-elastalert/
http://www.freebuf.com/articles/web/160254.html
http://www.xiaot123.com/post/elk_filebeat1