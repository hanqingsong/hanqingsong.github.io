---
title: 'Linux中curl: (35) SSL connect error问题'
date: 2018-06-04 19:07:48
categories:
    - 
tags:
---

无法在服务器使用curl命令访问https域名,原因是nss版本有点旧了，yum -y update nss更新一下，重新curl即可！