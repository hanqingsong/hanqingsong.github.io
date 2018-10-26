---
title: java中String的最长长度
date: 2018-09-20 14:37:16
categories:
    - java
tags:
    - 源码
---
网上好多答案是String的最大长度为Integer.MAX_VALUE，即2的31次方。

更深层次的答案是
《深入理解java虚拟机》中class类文件结构中有说明，在编译为字节码文件后，String类型的变量的值在常量池中使用CONSTANT_utf8-info类型表示，而CONSTANT_utf8-info型常量的结构为u1u2u1，得知长度限制使用两个字节的无符号二进制位表示，从而得出，String的最大长度为2的16次方，即65535。
65535只是在编译阶段的限制，运行中时，String对象的内部字符数组可以通过其它手段拼接成大于65535长度的字符串。所以运行时的字符串长度最多还应该是Integer.MAX_VALUE。

## 资料
《深入理解java虚拟机》
http://www.cnblogs.com/hanmou/p/4065147.html
