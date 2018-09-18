---
title: 'docker-part3:services'
date: 2017-10-11 09:45:11
categories:
    - docker
tags:
    - docker
---

## docker-compose.yml
docker-compose.yml定义了容器的行为。

```
version: "3"
services:
  web:
    # replace username/repo:tag with your name and image details
    image: username/repo:tag
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
networks:
  webnet:
```

## 运行负载均衡应用
```
➜  docker-test docker swarm init
Swarm initialized: current node (scxmr99cp0d6zjpajrckdktmk) is now a manager.

To add a worker to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-19wb8qe3cws960d8famp2lix8t1d6wza4eg9fw7geklefqjrvh-82wzkvoszligqto4eoe2ufail 192.168.65.2:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.

➜  docker-test docker stack deploy -c docker-compose.yml getstartedlab
Creating network getstartedlab_webnet
Creating service getstartedlab_web

➜  docker-test docker service ls
ID                  NAME                MODE                REPLICAS            IMAGE                             PORTS
lrd7bsarm31c        getstartedlab_web   replicated          5/5                 842071912/start-docker1:hellopy   *:80->80/tcp

➜  docker-test docker stack ls
NAME                SERVICES
getstartedlab       1

➜  docker-test docker service ps lrd7bsarm31c
ID                  NAME                  IMAGE                             NODE                DESIRED STATE       CURRENT STATE           ERROR               PORTS
pv0xalnz0jub        getstartedlab_web.1   842071912/start-docker1:hellopy   moby                Running             Running 2 minutes ago
qrkj0dctgukn        getstartedlab_web.2   842071912/start-docker1:hellopy   moby                Running             Running 2 minutes ago
xq97vphe37h5        getstartedlab_web.3   842071912/start-docker1:hellopy   moby                Running             Running 2 minutes ago
kc0kj4tdzlpz        getstartedlab_web.4   842071912/start-docker1:hellopy   moby                Running             Running 2 minutes ago
5roeqr1o8le3        getstartedlab_web.5   842071912/start-docker1:hellopy   moby                Running             Running 2 minutes ago

➜  docker-test docker container ls -q
3b010bc1ba11
e9c89868d2ee
34cc4a84736a
b697e352c205
92db487d0025

➜  docker-test docker stack rm getstartedlab
Removing service getstartedlab_web
Removing network getstartedlab_webnet

➜  docker-test docker node ls
ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS
scxmr99cp0d6zjpajrckdktmk *   moby                Ready               Active              Leader

➜  docker-test docker swarm leave --force
Node left the swarm.
➜  docker-test docker node ls
Error response from daemon: This node is not a swarm manager. Use "docker swarm init" or "docker swarm join" to connect this node to swarm and try again.

```

## 命令集
```
docker swarm init
docker stack ls                                            # List stacks or apps
docker stack deploy -c <composefile> <appname>  # Run the specified Compose file
docker service ls                 # List running services associated with an app
docker service ps <service>                  # List tasks associated with an app
docker inspect <task or container>                   # Inspect task or container
docker container ls -q                                      # List container IDs
docker stack rm <appname>                             # Tear down an application
```
