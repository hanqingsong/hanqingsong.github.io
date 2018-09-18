---
title: 'docker-part4:swarms'
date: 2017-10-11 10:16:18
categories:
    - docker
tags:
    - docker
---
## 设置swarm
mac系统下需要安装 [install Oracle VirtualBox](https://www.virtualbox.org/wiki/Downloads) 

### 创建集群
使用docker-machine创建VMs,创建有可能会失败多执行几次
```
➜  docker-test docker-machine ls
NAME   ACTIVE   DRIVER   STATE   URL   SWARM   DOCKER   ERRORS

➜  docker-test docker-machine create --driver virtualbox myvm1
Running pre-create checks...
(myvm1) No default Boot2Docker ISO found locally, downloading the latest release...
(myvm1) Latest release for github.com/boot2docker/boot2docker is v17.09.0-ce
(myvm1) Downloading /Users/hanqingsong/.docker/machine/cache/boot2docker.iso from https://github.com/boot2docker/boot2docker/releases/download/v17.09.0-ce/boot2docker.iso...
(myvm1) 0%....10%....20%....30%....40%....50%....60%....70%....80%....90%....100%
Creating machine...
(myvm1) Copying /Users/hanqingsong/.docker/machine/cache/boot2docker.iso to /Users/hanqingsong/.docker/machine/machines/myvm1/boot2docker.iso...
(myvm1) Creating VirtualBox VM...
(myvm1) Creating SSH key...
(myvm1) Starting the VM...
(myvm1) Check network to re-create if needed...
(myvm1) Found a new host-only adapter: "vboxnet0"
(myvm1) Waiting for an IP...
Waiting for machine to be running, this may take a few minutes...
Detecting operating system of created instance...
Waiting for SSH to be available...
Detecting the provisioner...
Provisioning with boot2docker...
Copying certs to the local machine directory...
Copying certs to the remote machine...
Setting Docker configuration on the remote daemon...
Checking connection to Docker...
Docker is up and running!
To see how to connect your Docker Client to the Docker Engine running on this virtual machine, run: docker-machine env myvm1

➜  docker-test docker-machine ls
NAME    ACTIVE   DRIVER       STATE     URL                         SWARM   DOCKER        ERRORS
myvm1   -        virtualbox   Running   tcp://192.168.99.100:2376           v17.09.0-ce
myvm2   -        virtualbox   Running   tcp://192.168.99.101:2376           v17.09.0-ce

```

### 初始化swarm添加节点
设置myvm1为swarm管理者
```
➜  docker-test docker-machine ssh myvm1 "docker swarm init --advertise-addr 192.168.99.100" 
Swarm initialized: current node (mwshg3zvljp17muqpuua5l3d1) is now a manager.

To add a worker to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-26jjkt2i3rul2qp7hwdp2m3ek1vegmumr6mk07ba8orc886ix2-c41i1zkhtnjqartdv6l75ms8h 192.168.99.100:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
```

myvm2添加为worker
```
➜  docker-test docker-machine ssh myvm2  "docker swarm join --token SWMTKN-1-26jjkt2i3rul2qp7hwdp2m3ek1vegmumr6mk07ba8orc886ix2-c41i1zkhtnjqartdv6l75ms8h 192.168.99.100:2377"
This node joined a swarm as a worker.
```

### 查看swarm节点
```
➜  docker-test docker-machine ssh myvm1 "docker node ls"
ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS
mwshg3zvljp17muqpuua5l3d1 *   myvm1               Ready               Active              Leader
urdtegb3yanqi0d98vzm49tof     myvm2               Ready               Active
```

## 在swarm集群部署应用

设置myvm1为激活状态
```
➜  docker-test docker-machine env myvm1
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://192.168.99.100:2376"
export DOCKER_CERT_PATH="/Users/hanqingsong/.docker/machine/machines/myvm1"
export DOCKER_MACHINE_NAME="myvm1"
# Run this command to configure your shell:
# eval $(docker-machine env myvm1)

➜  docker-test eval $(docker-machine env myvm1)

➜  docker-test docker-machine ls
NAME    ACTIVE   DRIVER       STATE     URL                         SWARM   DOCKER        ERRORS
myvm1   *        virtualbox   Running   tcp://192.168.99.100:2376           v17.09.0-ce
myvm2   -        virtualbox   Running   tcp://192.168.99.101:2376           v17.09.0-ce
```

在swarm manager部署应用
```
➜  docker-test docker stack deploy -c docker-compose.yml getstartedlab
Creating network getstartedlab_webnet
Creating service getstartedlab_web
```

查看服务部署信息
```
➜  docker-test docker stack ps getstartedlab
ID                  NAME                  IMAGE                             NODE                DESIRED STATE       CURRENT STATE                  ERROR               PORTS
qvs0h0pqgsou        getstartedlab_web.1   842071912/start-docker1:hellopy   myvm1               Running             Preparing about a minute ago
jtlfqsejqnmo        getstartedlab_web.2   842071912/start-docker1:hellopy   myvm2               Running             Running 24 seconds ago
wauh0t4cybvn        getstartedlab_web.3   842071912/start-docker1:hellopy   myvm1               Running             Preparing about a minute ago
feyi52iwjpa2        getstartedlab_web.4   842071912/start-docker1:hellopy   myvm2               Running             Running 24 seconds ago
m7mz2kevh15r        getstartedlab_web.5   842071912/start-docker1:hellopy   myvm2               Running             Running 24 seconds ago
```

## 清除和重启
 
 tear down the stack
 ```
 ➜  docker-test docker stack rm getstartedlab
Removing service getstartedlab_web
Removing network getstartedlab_webnet
 ```

>Keep the swarm or remove it?
At some point later, you can remove this swarm if you want to with docker-machine ssh myvm2 "docker swarm leave" on the worker and docker-machine ssh myvm1 "docker swarm leave --force" on the manager, but you’ll need this swarm for part 5, so please keep it around for now. 

```
➜  docker-test eval $(docker-machine env -u)


➜  docker-test docker-machine stop $(docker-machine ls -q)
Stopping "myvm2"...
Stopping "myvm1"...
Machine "myvm2" was stopped.
Machine "myvm1" was stopped.
➜  docker-test docker-machine ls
NAME    ACTIVE   DRIVER       STATE     URL   SWARM   DOCKER    ERRORS
myvm1   -        virtualbox   Stopped                 Unknown
myvm2   -        virtualbox   Stopped                 Unknown


```

## 命令
```
docker-machine create --driver virtualbox myvm1 # Create a VM (Mac, Win7, Linux)
docker-machine create -d hyperv --hyperv-virtual-switch "myswitch" myvm1 # Win10
docker-machine env myvm1                # View basic information about your node
docker-machine ssh myvm1 "docker node ls"         # List the nodes in your swarm
docker-machine ssh myvm1 "docker node inspect <node ID>"        # Inspect a node
docker-machine ssh myvm1 "docker swarm join-token -q worker"   # View join token
docker-machine ssh myvm1   # Open an SSH session with the VM; type "exit" to end
docker-machine ssh myvm2 "docker swarm leave"  # Make the worker leave the swarm
docker-machine ssh myvm1 "docker swarm leave -f" # Make master leave, kill swarm
docker-machine start myvm1            # Start a VM that is currently not running
docker-machine stop $(docker-machine ls -q)               # Stop all running VMs
docker-machine rm $(docker-machine ls -q) # Delete all VMs and their disk images
docker-machine scp docker-compose.yml myvm1:~     # Copy file to node's home dir
docker-machine ssh myvm1 "docker stack deploy -c <file> <app>"   # Deploy an app
```