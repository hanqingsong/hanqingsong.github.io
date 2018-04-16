---
title: mysql-cpu占用过高解决方法
date: 2018-04-03 11:01:09
categories: mysql
tags: mysql
---
CPU报警：很可能是 SQL 里面有较多的计算导致的
连接数超高：很可能是有慢查询，然后导致很多的查询在排队，排查问题的时候可以看到”事发现场“类似的 SQL 语句一大片，那么有可能是没有索引或者索引不好使，可以用：explain 分析一下 SQL 语句

## 查看事发现场
1. show processlist 语句，查找负荷最重的 SQL 语句，优化该SQL
2. 开启慢查询，找到性能瓶颈点的SQL语句，然后使用explain，看看该语句是否可以优化；

### show \[full\] processlist  
show \[full\] processlist  可以看到所有链接的情况
观察有问题的链接
```
-- 查询非 Sleep 状态的链接，按消耗时间倒序展示，自己加条件过滤
select id, db, user, host, command, time, state, info
from information_schema.processlist
where command != 'Sleep'
order by time desc 
```

## kill 使用

```
-- 查询执行时间超过2分钟的线程，然后拼接成 kill 语句
select concat('kill ', id, ';')
from information_schema.processlist
where command != 'Sleep'
and time > 2*60
order by time desc 
```