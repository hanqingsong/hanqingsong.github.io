---
title: 'docker-part5:Stacks'
date: 2017-10-11 13:46:28
categories:
    - docker
tags:
    - docker
---

## 新增一个服务（service）visualizer

修改docker-compose.yml
```
version: "3"
services:
  web:
    # replace username/repo:tag with your name and image details
    image: username/repo:tag
    deploy:
      replicas: 5
      restart_policy:
        condition: on-failure
      resources:
        limits:
          cpus: "0.1"
          memory: 50M
    ports:
      - "80:80"
    networks:
      - webnet
  visualizer:
    image: dockersamples/visualizer:stable
    ports:
      - "8080:8080"
    volumes:
      - "/var/run/docker.sock:/var/run/docker.sock"
    deploy:
      placement:
        constraints: [node.role == manager]
    networks:
      - webnet
networks:
  webnet:
```

更新服务
```
➜  docker-test docker stack deploy -c docker-compose.yml getstartedlab
Creating network getstartedlab_webnet
Creating service getstartedlab_web
Creating service getstartedlab_visualizer

➜  docker-test docker-machine ls
NAME    ACTIVE   DRIVER       STATE     URL                         SWARM   DOCKER        ERRORS
myvm1   *        virtualbox   Running   tcp://192.168.99.100:2376           v17.09.0-ce
myvm2   -        virtualbox   Running   tcp://192.168.99.101:2376           v17.09.0-ce
```
访问 http://192.168.99.100:8080/

## 持久化redis数据服务

修改docker-compose.yml
```
version: "3"
services:
  web:
    # replace username/repo:tag with your name and image details
    image: 842071912/start-docker1:hellopy
    deploy:
      replicas: 5
      resources:
        limits:
          cpus: "0.1"
          memory: 50M
      restart_policy:
        condition: on-failure
    ports:
      - "80:80"
    networks:
      - webnet
  visualizer:
    image: dockersamples/visualizer:stable
    ports:
      - "8080:8080"
    volumes:
      - "/var/run/docker.sock:/var/run/docker.sock"
    deploy:
      placement:
        constraints: [node.role == manager]
    networks:
      - webnet
  redis:
    image: redis
    ports:
      - "6379:6379"
    volumes:
      - /home/docker/data:/data
    deploy:
      placement:
        constraints: [node.role == manager]
    command: redis-server --appendonly yes
    networks:
      - webnet
networks:
  webnet:
```

创建./data文件夹
```
➜  docker-test docker-machine ssh myvm1 "mkdir ./data"

➜  docker-test docker stack deploy -c docker-compose.yml getstartedlab
Updating service getstartedlab_web (id: p3yof4klife0ulvn7fqwjrhkm)
Updating service getstartedlab_visualizer (id: s550x4sifn20ab391fsywu0l2)
Updating service getstartedlab_redis (id: a2eh1a3c3di608fsa00l7uvj8)

➜  docker-test docker service ls
ID                  NAME                       MODE                REPLICAS            IMAGE                             PORTS
a2eh1a3c3di6        getstartedlab_redis        replicated          1/1                 redis:latest                      *:6379->6379/tcp
s550x4sifn20        getstartedlab_visualizer   replicated          1/1                 dockersamples/visualizer:stable   *:8080->8080/tcp
p3yof4klife0        getstartedlab_web          replicated          5/5                 842071912/start-docker1:hellopy   *:80->80/tcp
```
访问 http://192.168.99.100:8080/