---
title: SpringBoot接收时间参数
categories:
  - SpringBoot
tags:
  - SpringBoot
  - spring
  - 后端
date: 2018-06-01 11:44:24
---

## 需求
服务端需要使用实体类属性字段接收时间，java.util.Date类型的属性字段。

## 问题
请求数据时间格式为："yyyy-MM-dd HH:mm:ss" 字符串
报错
```
{
    "timestamp": 1527824946500,
    "status": 400,
    "error": "Bad Request",
    "exception": "org.springframework.http.converter.HttpMessageNotReadableException",
    "message": "JSON parse error: Can not deserialize value of type java.util.Date from String \"2018-06-01 11:33:55\": not a valid representation (error: Failed to parse Date value '2018-06-01 11:33:55': Can not parse date \"2018-06-01 11:33:55\": while it seems to fit format 'yyyy-MM-dd'T'HH:mm:ss.SSS', parsing fails (leniency? null)); nested exception is com.fasterxml.jackson.databind.exc.InvalidFormatException: Can not deserialize value of type java.util.Date from String \"2018-06-01 11:33:55\": not a valid representation (error: Failed to parse Date value '2018-06-01 11:33:55': Can not parse date \"2018-06-01 11:33:55\": while it seems to fit format 'yyyy-MM-dd'T'HH:mm:ss.SSS', parsing fails (leniency? null))\n at [Source: java.io.PushbackInputStream@73f25959; line: 1, column: 228] (through reference chain: cn.xxx[\"rightsEndTime\"])",
    "path": "/grouppurchase/commodity/add"
}
```

1. Springboot使用的默认json解析框架是jackjson框架
2. jackjson解析框架在解析实体类里面是date数据类型的数据时的默认格式是：UTC类型，即yyyy-MM-dd'T'HH:mm:ss.SSS 并且默认为+8时区，即时间基础上加8小时

## 解决方案
实体Date类型的字段上使用@JsonFormat注解格式化日期 
```
@JsonFormat(locale="zh", timezone="GMT+8", pattern="yyyy-MM-dd HH:mm:ss")  
```

取消timestamps形式
```
objectMapper.configure(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS, false);  
```

fastjson解法方法
```
@JsonFormat(shape=JsonFormat.Shape.STRING, pattern="yyyy-MM-dd HH:mm:ss")  
```
