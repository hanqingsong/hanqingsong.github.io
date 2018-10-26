---
title: >-
  升级jdk1.8报错：sun.security.validator.ValidatorException: Certificate signature
  algorithm disabled
categories:
  - 问题记录
tags:
  - jdk
  - java
date: 2017-10-24 14:09:20
---


### 升级jdk1.8项目出错
sun.security.validator.ValidatorException: Certificate signature algorithm disabled

报错原因是JDK8对SSL证书的算法安全要求提高，可以对比下7和8的区别
```
less /Library/Java/JavaVirtualMachines/jdk1.7.0_80.jdk/Contents/Home/jre/lib/security/java.security|grep disabledAlgorithms |grep -v "#"
jdk.certpath.disabledAlgorithms=MD2, RSA keySize < 1024
jdk.tls.disabledAlgorithms=SSLv3

less /Library/Java/JavaVirtualMachines/jdk1.8.0_*/Contents/Home/jre/lib/security/java.security|grep disabledAlgorithms |grep -v "#"
jdk.certpath.disabledAlgorithms=MD2, MD5, SHA1 jdkCA & usage TLSServer, \
jdk.jar.disabledAlgorithms=MD2, MD5, RSA keySize < 1024
jdk.tls.disabledAlgorithms=SSLv3, RC4, MD5withRSA, DH keySize < 768, \
```

### 解决方法
1. 简单做法是直接注释掉jdk.certpath.disabledAlgorithms
2. 继承新的抽象类：X509ExtendedTrustManager
```
In the Java SE 7 release, the X509ExtendedTrustManager class is an abstract implementation of the X509TrustManager interface. It adds methods for connection-sensitive trust management. In addition, it enables endpoint verification at the TLS layer.
...
Besides TLS 1.2 support, the X509ExtendedTrustManager class also support algorithm constraints and SSL layer hostname verification.

```

## 参考
http://www.cnblogs.com/flyingeagle/articles/7508207.html
