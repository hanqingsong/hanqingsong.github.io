---
title: linux 安装node&npm
date: 2017-09-20 14:40:26
categories: 
- node
tags: 
- node
- npm
---

## 官网地址
[node官网](https://nodejs.org/zh-cn/download/)

### 安装步骤

```shell
sudo yum install gcc-c++ make

curl --silent --location https://rpm.nodesource.com/setup_8.x | sudo bash -

sudo yum -y install nodejs
```

说明：sudo bash 作为超级用户运行”bash”

### 查看版本

``` shell
node -v

npm -v
```

