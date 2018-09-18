---
title: Linux命令：crontab定时任务
date: 2018-04-16 09:45:18
categories: 
  - Linux
tags:
  - 命令
---
# Cron
cron其实是一个存放在/etc/init.d/下的一个脚本*crond*，随着系统开机自动启动，可以由service命令调度控制开启和关闭。

```
[root@VM_152_30_centos ~]# ll /etc/init.d/
total 236
-rwxr-xr-x  1 root root  1818 Feb 17  2016 acpid
-rwxr-xr-x  1 root root  2062 Feb 20  2015 atd
-rwxr-xr-x  1 root root  3580 May 11  2016 auditd
-r-xr-xr-x  1 root root  1343 Aug 24  2016 blk-availability
-rw-r--r--. 1 root root   460 Dec 25  2014 bootlocal
-rwxr-xr-x  1 root root  2826 Aug 24  2016 crond
```

## Step1
首先，cron会搜索/var/spool/cron/文件夹，这个文件夹下有多个以用户名命名的文件，每个文件就是属于各个用户的独立的cron配置文件。
```
[root@VM_152_30_centos ~]# cat /var/spool/cron/root
#secu-tcs-agent monitor, install at Fri Apr  1 15:51:35 CST 2016
* * * * * /usr/local/sa/agent/secu-tcs-agent-mon-safe.sh  > /dev/null 2>&1
*/20 * * * * /usr/sbin/ntpdate ntpupdate.tencentyun.com >/dev/null &
0 0 * * *  /usr/sbin/logrotate -vf /etc/logrotate.d/nginx
*/1 * * * * /usr/local/qcloud/stargate/admin/start.sh > /dev/null 2>&1 &
```

## Step2
然后，cron会去搜索/etc/crontab文件，并且解析里面的cron配置
```
[root@VM_152_30_centos ~]# cat /etc/crontab
SHELL=/bin/bash
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=root
HOME=/

# For details see man 4 crontabs

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name command to be executed

```
## Step3
最后，cron会去执行/etc/cron.d/这个文件夹下的东西，不过我们通常不建议在这里进行修改，虽然这个文件夹下的变化也会被监视，但是我们*更习惯将这种不通用的定时任务配置在/etc/crontab/里*。

# Crontab
crontab其实是方便用户来维护crontab配置文件的工具,通过crontab -l可以显示属于当前用户的/var/spool/cron/文件夹下的配置。通过crontab -e可以安全的进行编辑，如果语法不对他会进行提示，保证安全。
同时，crontab还提供了两个配置文件来控制用户的权限–/etc/cron.allow跟/etc/cron.deny。只有用户名在白名单里的用户才能使用crontab命令，用户名在黑名单里的用户是无法使用crontab命令的。显然，原则上这两个配置文件不能同时存在，如果同时存在，那么出于保守原则考虑，只有白名单有效，黑名单无效。如果这两个配置不存在，那么根据linux版本的不同，有的系统默认所有用户都有权限，有的系统默认只有root才有权限。

# 配置选择
## 固定用户的定时任务
我们就可以使用crontab -e命令去修改位于/var/spool/cron/下的属于当前用户的配置文件。这样就能够非常方便的区分不同用户的配置，保护了数据的安全。

## 固定时间的定时任务
很多情况下，作为系统管理员，我们需求的任务模式大都是每小时触发，每日触发，每周触发，每月触发之类的。那么这时我们就可以不用配置cron项，只需要把脚本放在对应的/etc/cron.daily,/etc/cron.hourly之类的文件夹下即可，方便省事。而且事实上，很多系统自身需要的定时任务就是这么办的。这种方式也是我们最推荐的方式，因为我们只要把需要定时执行的脚本放在规定的路径下即可，无需配置cron，毕竟cron配置文件用起来还是比shell脚本麻烦很多。

## 固定程序的定时任务
有时候，某些处理特定任务的进程也希望能够创建定时任务，比如我们编写或者安装的第三方任务。这些任务不希望依附于某一个用户，而希望拥有独立的配置文件，方便修改和卸载等等。这时候我们就可以新建一个cron配置文件，放置于/etc/cron.d/文件夹下，进行统一管理。像csf,lfd这类的进程就是这么做的，通过这样的配置保证服务定时重启。

# 资料
http://www.cnblogs.com/peida/archive/2013/01/08/2850483.html