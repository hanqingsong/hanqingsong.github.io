---
title: Linux 命令：压缩命令
categories:
  - Linux
tags:
  - 命令
date: 2017-10-14 09:45:11
---


1. tar命令
　　解包：tar zxvf FileName.tar
    解包到指定文件夹：tar zxvf FileName.tar -C dirname
　　打包：tar czvf FileName.tar DirName

2. gz命令
　　解压：gunzip FileName.gz
　　解压：gzip -d FileName.gz
    压缩：gzip FileName

3. tar.gz
    解压：tar zxvf FileName.tar.gz
　　 压缩：tar zcvf FileName.tar.gz DirName
    压缩多个文件：tar zcvf FileName.tar.gz DirName1 DirName2 DirName3

4. zip命令
　　解压：unzip FileName.zip
    解压到指定目录：unzip -d /home/sunny myfile.zip
　　压缩：zip -r FileName.zip DirName