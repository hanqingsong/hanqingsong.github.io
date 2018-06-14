---
title: git升级版本
date: 2018-06-04 18:59:07
categories:
    - git
tags:
    - git
---
在使用git pull、git push、git clone的时候，会报如下的错误： 
error: while accessing https://github.com/Yelp/elastalert.git/info/refs fatal: HTTP request failed
这个一般是由于服务器本身自带的git版本过低造成的：
```
git --version
git version 1.7.1
```

git版本升级
```
0）安装依赖软件
[root@uatjenkins01 ~]# yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel asciidoc
[root@uatjenkins01 ~]# yum install  gcc perl-ExtUtils-MakeMaker
 
1）卸载系统自带的底版本git（1.7.1）
[root@uatjenkins01 ~]# git --version
git version 1.7.1
[root@uatjenkins01 ~]# yum remove git
  
2）编译安装最新的git版本
[root@uatjenkins01 ~]# cd /usr/local/src/
[root@uatjenkins01 src]# wget https://mirrors.edge.kernel.org/pub/software/scm/git/git-2.17.1.tar.gz
[root@uatjenkins01 src]# tar -vxf git-2.17.1.tar.xz
[root@uatjenkins01 src]# cd git-2.17.1
[root@uatjenkins01 git-2.17.1]# make prefix=/usr/local/git all
[root@uatjenkins01 git-2.17.1]# make prefix=/usr/local/git install
[root@uatjenkins01 git-2.17.1]# echo "export PATH=$PATH:/usr/local/git/bin" >> /etc/profile
[root@uatjenkins01 git-2.17.1]# source /etc/profile
  
[root@uatjenkins01 ~]# git --version
git version 2.17.1
  
======================================================================
如果是非root用户使用git，则需要配置下该用户下的环境变量
[app@uatjenkins01 ~]$ echo "export PATH=$PATH:/usr/local/git/bin" >> ~/.bashrc
[app@uatjenkins01 ~]$ source ~/.bashrc
[app@uatjenkins01 ~]$ git --version
git version 2.17.1
```