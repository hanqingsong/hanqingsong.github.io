---
title: mac 安装使用Docker
date: 2017-10-09 14:28:36
categories:
    - docker
tags:
    - docker
---

## docker安装与启动
[docker官网地址](https://docs.docker.com/docker-for-mac/install/),下载dmg安装。
```
$ docker run hello-world
Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

$ docker --version
```

## 构建第一个app
[官网文档 Build app](https://docs.docker.com/get-started/part2/#apppy)

```
$ ls
Dockerfile      app.py          requirements.txt

$ docker build -t friendlyhello .
Sending build context to Docker daemon  4.608kB
Step 1/7 : FROM python:2.7-slim
2.7-slim: Pulling from library/python

$ docker images
REPOSITORY            TAG                 IMAGE ID
friendlyhello         latest              326387cea398

$ docker run -p 4000:80 friendlyhello 
或 docker run -d -p 4000:80 friendlyhello #-d 后台运行

```

访问http://localhost:4000。
CTRL+C退出

后台运行退出时
```
$ docker container ls #获取短CONTAINER ID
$ docker stop id
```

## 分享image

```
docker login
docker tag image username/repository:tag #Tag the image
比如：docker tag friendlyhello john/get-started:part2

docker images

docker push username/repository:tag #Publish the image

docker run -p 4000:80 username/repository:tag #Pull and run the image from the remote repository

```

## 容器&镜像命令列表

```
docker build -t friendlyname .  # Create image using this directory's Dockerfile
docker run -p 4000:80 friendlyname  # Run "friendlyname" mapping port 4000 to 80
docker run -d -p 4000:80 friendlyname         # Same thing, but in detached mode
docker container ls                                # List all running containers
docker container ls -a             # List all containers, even those not running
docker container stop <hash>           # Gracefully stop the specified container
docker container kill <hash>         # Force shutdown of the specified container
docker container rm <hash>        # Remove specified container from this machine
docker container rm $(docker container ls -a -q)         # Remove all containers
docker image ls -a                             # List all images on this machine
docker image rm <image id>            # Remove specified image from this machine
docker image rm $(docker image ls -a -q)   # Remove all images from this machine
docker login             # Log in this CLI session using your Docker credentials
docker tag <image> username/repository:tag  # Tag <image> for upload to registry
docker push username/repository:tag            # Upload tagged image to registry
docker run username/repository:tag                   # Run image from a registry
```

## 镜像的获取

```
# 搜索镜像
docker search <image> # 在docker index中搜索image
# 下载镜像
docker pull <image>  # 从docker registry server 中下拉image
# 查看镜像 
docker images： # 列出images
docker images -a # 列出所有的images（包含历史）
docker rmi  <image ID>： # 删除一个或多个image
```

## 容器的使用

```
# 查看容器
    docker ps ：列出当前所有正在运行的container
    docker ps -l ：列出最近一次启动的container
    docker ps -a ：列出所有的container（包含历史，即运行过的container）
    docker ps -q ：列出最近一次运行的container ID
# 再次启动容器
    docker start/stop/restart <container> #：开启/停止/重启container
    docker start [container_id] #：再次运行某个container （包括历史container）
#进入正在运行的docker容器
    docker exec -it [container_id] /bin/bash
    docker run -i -t -p <host_port:contain_port> #：映射 HOST 端口到容器，方便外部访问容器内服务，host_port 可以省略，省略表示把 container_port 映射到一个动态端口。

# 删除容器
    docker rm <container...> #：删除一个或多个container
    docker rm `docker ps -a -q` #：删除所有的container
    docker ps -a -q | xargs docker rm #：同上, 删除所有的container
```