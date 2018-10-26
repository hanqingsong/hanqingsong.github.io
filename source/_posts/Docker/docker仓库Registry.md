---
title: docker仓库Registry
categories:
  - docker
tags:
  - docker
date: 2018-05-10 15:55:45
---

仓库Registry 方便保存和分发镜像，Docker Hub 是 Docker 公司维护的公共 Registry。用户可以将自己的镜像保存到 Docker Hub 免费的 repository 中。

1. Docker Hub 上注册一个账号
https://hub.docker.com/

2. 登录
注意使用docker id登录，不要使用邮箱登录。不然会报错
```
Error response from daemon: Get https://registry-1.docker.io/v2/: unauthorized: incorrect username or password
```

3. docker tag 命令重命名镜像
```
docker tag centos-vim-f 842071912/centos-vim-f:v1
```

4. docker push 842071912/centos-vim-f
5. docker pull 拉取镜像
