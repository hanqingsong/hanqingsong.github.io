---
title: joda-time工具类
categories:
  - java
tags:
  - joda
  - java
date: 2017-12-04 09:38:00
---

<!-- MarkdownTOC -->

- [数据初始化](#%E6%95%B0%E6%8D%AE%E5%88%9D%E5%A7%8B%E5%8C%96)
- [日期差](#%E6%97%A5%E6%9C%9F%E5%B7%AE)
- [格式化](#%E6%A0%BC%E5%BC%8F%E5%8C%96)
- [日期计算 plus/minus](#%E6%97%A5%E6%9C%9F%E8%AE%A1%E7%AE%97-plusminus)
- [时间比较 isBefore/isAfter](#%E6%97%B6%E9%97%B4%E6%AF%94%E8%BE%83-isbeforeisafter)
- [官网](#%E5%AE%98%E7%BD%91)

<!-- /MarkdownTOC -->

## 数据初始化
```
DateTime dateTimeNow = new DateTime();
DateTime dateTimeNow = new DateTime(date);//java日期转DateTime
DateTime recent = DateTime.parse("2017-05-18 17:40:00", DateTimeFormat.forPattern("yyyy-MM-dd HH:mm:ss")); //字符串转日期
DateTime dateTime2 = new DateTime(2017,2,14,0,0,0);//年月日时分秒
DateTime dateTime = DateTimeFormat.forPattern("yyyy-MM-dd HH:mm:ss").parseDateTime("2016-06-03 23:59:59");
```


## 日期差 

```
DateTime dt1 = new DateTime();
DateTime dt2 = new DateTime().plus();
System.out.print("时间相差：");
System.out.print(Days.daysBetween(dt1, dt2).getDays() + " 天 ");
System.out.print(Hours.hoursBetween(dt1, dt2).getHours() % 24 + " 小时 ");
System.out.print(Minutes.minutesBetween(dt1, dt2).getMinutes() % 60 + " 分钟 ");
System.out.print(Seconds.secondsBetween(dt1, dt2).getSeconds() % 60+ " 秒.");
```

## 格式化
```
 String s = new DateTime().toString(DateTimeFormat.forPattern("yyyy-MM-dd"));
 String s2 = new DateTime().toString("yyyy-MM-dd")
```

## 日期计算 plus/minus
```
DateTime now = new DateTime();//当前时间
DateTime tomorrowDt = now.plusDays(1);//明天

int days = Days.daysBetween(now, tomorrowDt).getDays();
System.out.println(days);//1
```

## 时间比较 isBefore/isAfter
时间轴

```
现在 isbefore 明天 true
boolean before = new DateTime().isBefore(new DateTime().plus(1));
```

##官网
http://www.joda.org/joda-time/userguide.html

