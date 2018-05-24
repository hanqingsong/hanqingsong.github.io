---
title: docker安装和使用
date: 2018-05-07 22:34:05
categories:
    - docker
tags:
    - docker
---

## 安装
```
# yum -y install docker
# docker --version
Docker version 1.13.1, build 774336d/1.13.1
```

## 启动
```
[root@VM_0_8_centos ~]# service docker start
Redirecting to /bin/systemctl start  docker.service
[root@VM_0_8_centos ~]# docker info


[root@VM_0_8_centos ~]# docker container --help
```

## 运行状态
```
[root@VM_0_8_centos ~]# service docker status
```

## hello-world
```
[root@VM_0_8_centos ~]# docker run hello-world


[root@VM_0_8_centos ~]# docker image ls
REPOSITORY              TAG                 IMAGE ID            CREATED             SIZE
docker.io/hello-world   latest              e38bc07ac18e        3 weeks ago         1.85 kB


[root@VM_0_8_centos ~]# docker container ls --all
CONTAINER ID        IMAGE               COMMAND             CREATED              STATUS                          PORTS               NAMES
298b7dfcf12d        hello-world         "/hello"            About a minute ago   Exited (0) About a minute ago                       affectionate_williams
```
docker run过程：
1.首先本地查找
2.本地没有，从Docker Hub或系统配置的默认Registry中下载

## 容器管理
## docker ps
```
[root@VM_0_8_centos ~]# docker help ps

Usage:  docker ps [OPTIONS]

List containers

Options:
  -a, --all             Show all containers (default shows just running)
  -f, --filter filter   Filter output based on conditions provided
      --format string   Pretty-print containers using a Go template
      --help            Print usage
  -n, --last int        Show n last created containers (includes all states) (default -1)
  -l, --latest          Show the latest created container (includes all states)
      --no-trunc        Don't truncate output
  -q, --quiet           Only display numeric IDs
  -s, --size            Display total file sizes
```

### docker run
如果我们需要一个保持运行的容器呢，最简单的方法就是给这个容器一个可以保持的应用，比如bash，运行 ubuntu 容器并进入容器的 bash
```
$ docker run -t -i ubuntu /bin/bash
-t：分配一个 pseudo-TTY
-i：--interactive参数缩写，表示交互模式，如果没有 attach 保持 STDIN 打开状态
ubuntu：运行的镜像名称，默认为latest 标签
/bin/bash：容器中运行的应用

退出
1. 直接 exit，这时候 bash 程序终止，容器进入到停止状态
2. 使用组合键退出，仍然保持容器运行，我们可以随时回来到这个bash中来，组合键是 Ctrl-p Ctrl-q，你没有看错，是两组组合键，先同时按下Ctrl和p，再按Ctrl和q。就可以退出到我们的宿主机了。

```
docker run 命令实际上是 docker create 和 docker start 的组合。

### docker attach
进入正在运行的容器
可通过 Ctrl+p 然后 Ctrl+q 组合键退出 attach 终端。

### docker exec
docker exec 进入相同的容器

### attach VS exec
attach 与 exec 主要区别如下:
1. attach 直接进入容器 启动命令 的终端，不会启动新的进程。
2. exec 则是在容器中打开新的终端，并且可以启动新的进程。
3. 如果想直接在终端中查看启动命令的输出，用 attach；其他情况使用 exec。

### docker start
启动容器

### docker inspect
查看 Docker 容器或镜像的一些内部信息

### docker rm 
删除容器操作。不能删除运行的容器。
一次可以指定多个容器，如果希望批量删除所有已经退出的容器，可以执行如下命令：
docker rm -v $(docker ps -aq -f status=exited)

## 镜像管理
### docker images
列出当前系统上所有的镜像信息

### docker search
搜索镜像 

### docker pull
获取镜像

### history   
显示镜像构建历史

### tag       
给镜像打 tag

### Dockerfile 创建镜像
```
1. 创建Dockerfile文件
from ubuntu:latest
ENV HOSTNAME=shiyanlou
2. 执行docker build -t 镜像名 Dockerfile文件目录
3. 查看docker images

```

### docker inspect
查看 Docker 容器或镜像的一些内部信息

### docker rmi 
清理镜像

### search    
搜索 Docker Hub 中的镜像
 
## 网络管理
### 默认情况
```
Docker服务启动时会自动创建一个 docker0 的虚拟网桥，后续新创建的容器都会有个虚拟接口连接到这个网桥
1. NAT模式
2. 网址自动分配
```

### 配置文件
```
/etc/default/docker

如果需要对网络进行配置，则需要对配置参数DOCKER_OPTS启动参数进行修改

网络配置的相关参数
1. -b --bridge ：指定连接的网桥
2. --bip=CIDR：指定IP地址网段
3. --icc=true|false：是否允许容器间网络互通
4. --ip-forward=true|false：是否允许IP转发，可以对容器的外网访问进行限制 修改docker配置文件后需要重启docker服务
```

### 容器启动时网络参数设置
```
1. -h --hostname：配置容器主机名
2. --link：添加另外一个容器的链接，见后续实验内容
3. --net=bridge|none|container|host：设置容器的网络模式

```

### 限制容器访问外网
限制容器访问外网，可以关闭IP转发，设置方法是启动Docker时--ip-forward=false。
或sudo vim /etc/default/docker 修改配置文件

### 限制容器间的访问
限制容器间的访问，可以设置--icc参数，或设置iptables参数。默认容器间可以互相访问，通过设置--icc=false可以禁止。

### Docker 容器端口映射
docker run -ti -p 80:80 -p 5000:5000 --name shiyanlou3 ubuntu
宿主机端口:容器端口
如果想映射到指定的本地地址，可以增加IP参数，比如映射到127.0.0.1地址，只需要将参数写成 -p 127.0.0.1:80:80。
docker port 容器名查看端口映射情况

### Docker 容器互联
```
docker run -d --name db redis
docker run -ti --nam app --link db:db shiyanloutest:1.0 /bin/bash
docker run -ti --name web --link app:app shiyanloutest:1.0 /bin/bash
```
通过link连接

## 数据卷的使用
1. 使用docker create命令创建但不启动数据卷容器：
```
docker create -v /shiyanloudata --name shiyanloudb ubuntu /bin/true
```

2. 其他使用该数据卷容器的容器创建时候需要使用--volumes-from参数，指定该容器名称或ID：
```
docker run --volumes-from shiyanloudb ...
#s1 s2容器中都将会有一个/shiyanloudata目录，二者是共享目录的
docker run --volumes-from shiyanloudb --name s1 -t -i  ubuntu /bin/bash
docker run --volumes-from shiyanloudb --name s1 -t -i  ubuntu /bin/bash
```

### 备份数据卷
```
docker run --rm --volumes-from shiyanloudb -v /tmp/backup:/backup ubuntu tar cvf /backup/shiyanloudb.tar /shiyanloudata
```

命令解析
1. docker run --rm --volumes-from shiyanloudb 创建并在执行操作后移除容器
2. -v /tmp/backup:/backup 将创建的容器的backup目录挂载在宿主的/tmp/backupm目录
3. 将数据卷 /shiyanloudata 的数据打包shiyanloudb.tar并存放到/backup/shiyanloudb.tar
执行完上述命令，在宿主机的/tmp/backup/shiyanloudb.tar下将会有shiyanloudb.tar文件

## 资料
https://docs.docker.com/install/linux/docker-ce/centos/#os-requirements
https://docs.docker.com/get-started/