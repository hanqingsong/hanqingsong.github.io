---
title: jvm 工具
date: 2017-10-09 10:34:46
categories:
    - JVM
tags:
    - JVM
---

## 常用工具

jps：主要用来输出JVM中运行的进程状态信息 (Java Virtual Machine Process Status Tool)、
jstack：主要用来查看某个Java进程内的线程堆栈信息
jmap：生成堆快照
jstat: 显示进程中的类装载、内存、垃圾收集、JIT编译等运行数据
jhat：html形式显示对象占用内存大小以及引用情况
jconsole（图形工具）
jvisualvm（图形工具）
MAT （第三方图形工具）

## jps
jps主要用来输出JVM中运行的进程状态信息。语法格式如下：
>jps [options] [hostid]
-q 不输出类名、Jar名和传入main方法的参数
-m 输出传入main方法的参数
-l 输出main类或Jar的全限名
-v 输出传入JVM的参数

## jstack
jstack主要用来查看某个Java进程内的线程堆栈信息。语法格式如下：
>jstack [option] pid
jstack [option] executable core
jstack [option] [server-id@]remote-hostname-or-ip
-l long listings，会打印出额外的锁信息，在发生死锁时可以用jstack -l pid来观察锁持有情况
-m mixed mode，不仅会输出Java堆栈信息，还会输出C/C++堆栈信息（比如Native方法）

## jmap

>jmap -heap :当前堆内存分布信息，如From space, To Space等占用内存大小
jmap -histo : 当前堆中对象占用内存大小情况，柱状图数据结构组织。可以简单得定位下当前占用内存最大的几个对象
jmap -histo:live:先触发一次gc , 再统计对象占用内存情况。可以简单得定位下当前占用内存最大的几个对象以及对象是否可以被gc回收
jmap -dump:format=b,file=heapDump: 导出堆详细使用信息，b表示二进制文件，之后采用其他工具分析，如jhat,mat.非常详细，可分析到对象之间的引用关系等。

## jstat
显示进程中的类装载、内存、垃圾收集、JIT编译等运行数据
目前感觉用上的都是gc记录查看，实际还没太用过
>jstat -gc 3331 250 20: 查询进程2764的垃圾收集情况，每250毫秒查询一次，一共查询20次。
jstat -gcause

>S0C : survivor0区的总容量
S1C : survivor1区的总容量
S0U : survivor0区已使用的容量
S1U : survivor1区已使用的容量
EC : Eden区的总容量
EU : Eden区已使用的容量
OC : Old区的总容量
OU : Old区已使用的容量
PC  当前perm的容量 (KB)
PU  perm的使用 (KB)
YGC : 新生代垃圾回收次数
YGCT : 新生代垃圾回收时间
FGC : 老年代垃圾回收次数
FGCT : 老年代垃圾回收时间
GCT : 垃圾回收总消耗时间


## jhat
可用来分析 jmap dump生成的堆信息二进制文件。html形式显示对象占用内存大小以及引用情况，但显示得并不友好。感觉比较鸡肋。

>jhat file

## jconsole（图形工具）
Jconsole（Java Monitoring and Management Console）从java5开始，在JDK中自带的java监控和管理控制台，用于对JVM中内存，线程和类等的监控

## jvisualvm（图形工具）
用于查看 Java 虚拟机 (Java Virtual Machine, JVM) 上运行的基于 Java 技术的应用程序（Java 应用程序）的详细信息。

## MAT
MAT(Memory Analyzer Tool)，一个基于Eclipse的内存分析工具，是一个快速、功能丰富的Java heap分析工具，它可以帮助我们查找内存泄漏和减少内存消耗。

## 资料

https://my.oschina.net/feichexia/blog/196575
http://www.jianshu.com/p/6bbab921102b
http://www.ityouknow.com/ 
