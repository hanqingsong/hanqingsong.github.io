---
title: mysql 流程控制语句if/case
date: 2017-09-25 11:50:43
categories: 
- mysql
tags:
- mysql
- sql
---

## if表达式
>IF(expr1,expr2,expr3)

```
select if(v=1,'男','女') from t1 ;

```

如果条件判断多可以使用case when then。

## case

```
CASE   <单值表达式>
        WHEN <表达式值> THEN <SQL语句或者返回值>
        WHEN <表达式值> THEN <SQL语句或者返回值>
        WHEN <表达式值> THEN <SQL语句或者返回值>
        ELSE <SQL语句或者返回值>
 END
```

### 简单Case函数写法

```
select case v 
    when 1 then '男'
    when 0 then '女'
    end
 from t1 ;
```

### Case搜索函数写法

```
select case  
    when v=1 then '男'
    when v=0 then '女'
    end
 from t1 ;
```

## 参考
http://www.cnblogs.com/martinzhang/p/3220595.html
