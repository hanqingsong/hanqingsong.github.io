---
title: java多线程中Sleep与Wait的区别
date: 2018-10-26 12:16:12
categories:
    - java
tags:
    - java
---
java中的多线程是一种`抢占式`的机制而不是分时机制。抢占式机制指的是有多个线程处于可运行状态，但是只有一个线程在运行。 

## 共同点： 
1. 他们都是在多线程的环境下，都可以在程序的调用处阻塞指定的毫秒数，并返回。 
2. wait()和sleep()都可以通过interrupt()方法打断线程的暂停状态，从而使线程立刻抛出InterruptedException。 

如果线程A希望立即结束线程B，则可以对线程B对应的Thread实例调用interrupt方法。如果此刻线程B正在wait/sleep /join，则线程B会立刻抛出InterruptedException，在catch() {} 中直接return即可安全地结束线程。 
需要注意的是，InterruptedException是线程自己从内部抛出的，并不是interrupt()方法抛出的。对某一线程调用 interrupt()时，如果该线程正在执行普通的代码，那么该线程根本就不会抛出InterruptedException。但是，一旦该线程进入到 wait()/sleep()/join()后，就会立刻抛出InterruptedException 。 

## 不同点： 
1. Thread类的方法：sleep(),yield()等 
   Object的方法：wait()和notify()等 
   
2. 每个对象都有一个锁来控制同步访问。Synchronized关键字可以和对象的锁交互，来实现线程的同步。 sleep方法没有释放锁，而wait方法释放了锁，使得其他线程可以使用同步控制块或者方法。 

3. wait，notify和notifyAll只能在同步控制方法或者同步控制块里面使用，而sleep可以在任何地方使用 

4. sleep必须捕获异常，而wait，notify和notifyAll不需要捕获异常  

## 资料
http://uule.iteye.com/blog/1106710
https://www.journaldev.com/1020/thread-sleep-java

