---
title: nginx添加日志分割logrotate
categories:
  - nginx
tags:
  - nginx
  - null
date: 2017-10-14 09:45:11
---

## logrotate简介
logrotate 是Linux系统日志文件管理工具。用来把旧的日志文件删除，并创建新的日志文件，我们把它叫做“转储”。可以根据日志文件的大小，也可以根据其天数来转储，这个过程一般通过 cron 程序来执行。
logrotate 程序还可以用于压缩日志文件，以及发送日志到指定的E-mail。
默认的logrotate被加入cron的/etc/cron.daily中作为每日任务执行。
/etc/logrotate.d/* 为/etc/logrotate.conf默认包含目录其中文件也会被logrotate读取。指明每个日志文件的特定规则。
/var/lib/logrotate/status中默认记录logrotate上次轮换日志文件的时间。

## logrotate安装
centos是默认有安装
yum install logrotate
安装完成之后在，在/etc/logrotate.d/有以下脚本
```
$ ll /etc/logrotate.d/
total 24
drwxr-xr-x. 2 root root 4096 Mar  3  2016 2016-03-03
-rw-r--r--. 1 root root  103 Jul 14  2015 dracut
-rw-r--r--  1 root root  217 Oct 31  2016 nginx
-rw-r--r--. 1 root root  329 Jul 17  2012 psacct
-rw-r--r--. 1 root root  265 Mar  3  2016 syslog
-rw-r--r--. 1 root root  100 Feb 22  2013 yum

```

## 运行机制
crontab会每天定时执行/etc/cron.daily目录下的脚本，在这个目录有个logrotate脚本，脚本内容为
```
#!/bin/sh

/usr/sbin/logrotate /etc/logrotate.conf >/dev/null 2>&1
EXITVALUE=$?
if [ $EXITVALUE != 0 ]; then
    /usr/bin/logger -t logrotate "ALERT exited abnormally with [$EXITVALUE]"
fi
exit 0
```
每天执行一次/usr/sbin/logrotate /etc/logrotate.conf，logrotate.conf中`include /etc/logrotate.d` 

## nginx脚本
```
/var/log/nginx/*log {
    daily
    rotate 10
    missingok
    notifempty
    compress
    sharedscripts
    postrotate
        /bin/kill -USR1 $(cat /var/run/nginx.pid 2>/dev/null) 2>/dev/null || :
    endscript
}
```

```
compress                        通过gzip压缩转储以后的日志
nocompress                      不压缩
copytruncate                    用于还在打开中的日志文件，把当前日志备份并截断
nocopytruncate                  备份日志文件但是不截断
create mode owner group         转储文件，使用指定的文件模式创建新的日志文件
nocreate                        不建立新的日志文件
delaycompress 和 compress        一起使用时，转储的日志文件到下一次转储时才压缩
nodelaycompress                 覆盖 delaycompress 选项，转储同时压缩。
errors address                   专储时的错误信息发送到指定的Email 地址
ifempty                         即使是空文件也转储，这个是 logrotate 的缺省选项。
notifempty                      如果是空文件的话，不转储
mail address                    把转储的日志文件发送到指定的E-mail 地址
nomail                          转储时不发送日志文件
olddir directory                转储后的日志文件放入指定的目录，必须和当前日志文件在同一个文件系统
noolddir                        转储后的日志文件和当前日志文件放在同一个目录下
prerotate/endscript             在转储以前需要执行的命令可以放入这个对，这两个关键字必须单独成行
postrotate/endscript            在转储以后需要执行的命令可以放入这个对，这两个关键字必须单独成行
daily                           指定转储周期为每天
weekly                          指定转储周期为每周
monthly                         指定转储周期为每月
rotate count                    指定日志文件删除之前转储的次数，0 指没有备份，5 指保留5 个备份
tabootext [+] list 让logrotate   不转储指定扩展名的文件，缺省的扩展名是：.rpm-orig, .rpmsave, v, 和 ~ 
size size                       当日志文件到达指定的大小时才转储，bytes(缺省)及KB(sizek)或MB(sizem)

```

## 命令
立刻执行分割：/usr/sbin/logrotate -vf  /etc/logrotate.d/nginx
查看执行状态 cat /var/lib/logrotate.status

## 添加定时任务
```
crontab -e 
#添加以下代码， #每天凌晨定时执行脚本
# crond-id-02:nginx_logs logroate
0 0 * * * /usr/sbin/logrotate -vf /etc/logrotate.d/nginx >/dev/null 2>&1
```


