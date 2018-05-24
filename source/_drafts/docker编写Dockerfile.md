---
title: docker编写Dockerfile
date: 2018-05-07 22:42:43
categories:
    - docker
tags:
    - docker
---
## Dockerfile
Dockerfile定义在container里有什么，Dockerfile 是一个包含创建镜像所有命令的文本文件，通过docker build命令可以根据 Dockerfile 的内容构建镜像。
```
指令选项:
FROM
MAINTAINER
RUN
CMD
EXPOSE
ENV
ADD
COPY
ENTRYPOINT
VOLUME
USER
WORKDIR
ONBUILD
```

### FROM
FROM <image> 
FROM指定构建镜像的基础源镜像，如果本地没有指定的镜像，则会自动从 Docker 的公共库 pull 镜像下来。
FROM必须是 Dockerfile 中非注释行的第一个指令，即一个 Dockerfile 从FROM语句开始。
FROM可以在一个 Dockerfile 中出现多次，如果有需求在一个 Dockerfile 中创建多个镜像。
如果FROM语句没有指定镜像标签，则默认使用latest标。

### MAINTAINER
MAINTAINER <name>
指定创建镜像的用户

### RUN
RUN
RUN "executable", "param1", "param2"
每条RUN指令将在当前镜像基础上执行指定命令，并提交为新的镜像，后续的RUN都在之前RUN提交后的镜像为基础，镜像是分层的，可以通过一个镜像的任何一个历史提交点来创建，类似源码的版本控制。

### CMD
CMD有三种使用方式:
    CMD "executable","param1","param2"
    CMD "param1","param2"
    CMD command param1 param2 (shell form)
CMD的目的是为了在启动容器时提供一个默认的命令执行选项。如果用户启动容器时指定了运行的命令，则会覆盖掉CMD指定的命令。
CMD会在`启动容器`的时候执行，build 时不执行，而RUN只是在`构建镜像`的时候执行，后续镜像构建完成之后，启动容器就与RUN无关了，这个。

### EXPOSE
EXPOSE <port> [<port>...]
告诉 Docker 服务端容器对外映射的本地端口，需要在 docker run 的时候使用-p或者-P选项生效。

### ENV
ENV <key> <value>       # 只能设置一个变量
ENV <key>=<value> ...   # 允许一次设置多个变量
指定一个环节变量，会被后续RUN指令使用，并在容器运行时保留。

### ADD
ADD <src>... <dest>
ADD复制本地主机文件、目录或者远程文件 URLS 从 并且添加到容器指定路径中 。

### COPY
COPY <src>... <dest>
COPY复制新文件或者目录从 并且添加到容器指定路径中 。用法同ADD，唯一的不同是不能指定远程文件 URLS。

### ENTRYPOINT
ENTRYPOINT "executable", "param1", "param2"
ENTRYPOINT command param1 param2 (shell form)
配置容器启动后执行的命令，并且不可被 docker run 提供的参数覆盖，而CMD是可以被覆盖的。如果需要覆盖，则可以使用docker run --entrypoint选项

### VOLUME
VOLUME ["/data"]
创建一个可以从本地主机或其他容器挂载的挂载点

### USER
USER daemon
指定运行容器时的用户名或 UID，后续的RUN、CMD、ENTRYPOINT也会使用指定用户。

### WORKDIR
WORKDIR /path/to/workdir
为后续的RUN、CMD、ENTRYPOINT指令配置工作目录。可以使用多个WORKDIR指令，后续命令如果参数是相对路径，则会基于之前命令指定的路径。

### ONBUILD
ONBUILD [INSTRUCTION]
配置当所创建的镜像作为其它新创建镜像的基础镜像时，所执行的操作指令。

## 实例
```
# Version 0.1
# 基础镜像
FROM nbuntu:latest
# 维护者信息
MAINTAINER lihong/leehongitrd@163.com 
# 镜像操作命令
RUN apt-get -yqq update && apt-get install -yqq apache2 && apt-get clean
# 容器启动命令
CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
```
上面的Dockerfile就做了一件事，即创建一个apache的镜像,
1. FROM指定基础镜像，如果镜像名称中没有制定TAG, 默认为latest。
2. RUN命令默认使用/bin/sh Shell执行，默认为root权限。如果命令过长需要换行，需要在行末尾加\\。
3. CMD 命令也是默认在/bin/sh Shell中执行，并且默认只能有一条，如果是多条CMD命令则只有最后一条执行。用户也可以在docker run命令创建容器时指定新的CMD命令来覆盖Dockerfile里的CMD。

用docker build将上述Dockerfile构建名为test:0.1的新镜像,
```
docker build -t test:0.1 .
```

使用该镜像创建容器web1, 将容器中的端口80映射到本地80端口,
```
docker run -d -p 80:80 --name web1 test:0.1
```
