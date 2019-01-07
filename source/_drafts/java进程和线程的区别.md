---
title: java进程和线程的区别
date: 2018-10-26 10:29:08
categories:
    - java
tags:
    - java
---
进程和线程的区别
## 进程和线程的区别
进程：进程是程序的一次运行过程，是一个动态概念，是程序在执行过程中分配和管理资源的基本单位。每个进程有自己的地址空间，至少有5种基本状态：初始态、运行态、等待状态、就绪状态、终止状态。
线程：线程是cpu调度和分派的基本单位，它可同属于一个进程的其他线程共享进程所拥有的全部资源。
联系：线程是进程的一部分，一个进程可以有多个线程，一个线程只属于一个进程。
区别：根本区别是进程是操作系统资源分配的基本单位，而线程是任务调度和执行的基本单位。

## 线程的五种状态
1 New
```
When we create a new Thread object using new operator, thread state is New Thread. At this point, thread is not alive and it’s a state internal to Java programming.
```
2 Runnable
```
When we call start() function on Thread object, it’s state is changed to Runnable. The control is given to Thread scheduler to finish it’s execution. Whether to run this thread instantly or keep it in runnable thread pool before running, depends on the OS implementation of thread scheduler.

```
3 Running
```
When we call start() function on Thread object, it’s state is changed to Runnable. The control is given to Thread scheduler to finish it’s execution. Whether to run this thread instantly or keep it in runnable thread pool before running, depends on the OS implementation of thread scheduler.

```
4 Blocked/Waiting
```
A thread can be waiting for other thread to finish using thread join or it can be waiting for some resources to available. For example producer consumer problem or waiter notifier implementation or IO resources, then it’s state is changed to Waiting. Once the thread wait state is over, it’s state is changed to Runnable and it’s moved back to runnable thread pool.
```

5 Dead
```
Once the thread finished executing, it’s state is changed to Dead and it’s considered to be not alive.
```

## 程的状态转换
调用 start()后的可运行（就绪 Runnable）状态！
sleep() ->  阻塞状态  -》 停止几秒
join()  -》 阻塞状态 -》等待前一线程执行完
wait() -》 释放锁和资源，进入等待队列  -》 notify()唤醒后进入锁池
synchronized  -》 获取不到锁，进入锁池

## 参考资料

http://uule.iteye.com/blog/1100799
