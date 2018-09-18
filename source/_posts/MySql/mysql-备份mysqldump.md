---
title: mysql 备份mysqldump
date: 2017-09-25 12:08:35
categories:
    - mysql
tags:
    - mysql
    - 备份数据库
---

## 备份数据库的某表数据

```
mysqldump -u root -p database_name  table_name table_name2 > database_dump.txt
```

## 备份整个数据库的数据

```
mysqldump -u root -p database_name > database_dump.txt
```

## 备份所有数据库

```
mysqldump -u root -p --all-databases > database_dump.txt
```