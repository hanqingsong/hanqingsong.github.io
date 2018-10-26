---
title: jvm方法区内存溢出
categories:
  - JVM
tags:
  - JVM
date: 2018-07-11 10:24:45
---

java.lang.OutOfMemoryError: PermGen space

* jps查看java程序pid
* 保存heap：jmap -dump:format=b,file=heap.hprof pid，之后使用java自带工具jvisualvm分析数据
* top查看cpu占用情况
* top -H -p pid查看该进程具体线程情况 或者ps -mp pid -o THREAD,did,time
* 查看问题线程堆栈 ，查看该线程的堆栈情况，先将线程id转为16进制，使用printf "%x\n" tid命令进行转换，再使用jstack命令打印线程堆栈信息，命令格式：jstack pid |grep tid -A 30
* jstat -gcutil pid 2000 10 命令查看进程的内存情况
* jstack命令查看进程的堆栈情况