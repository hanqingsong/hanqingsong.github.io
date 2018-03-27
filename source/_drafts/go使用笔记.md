---
title: go使用笔记
date: 2018-03-20 09:41:01
categories: 
    - go
tags: 
    - go
---
## 安装Golang

```
brew install go
```

## 配置GOPATH

```
➜  ~ go env
GOARCH="amd64"
GOBIN=""
GOEXE=""
GOHOSTARCH="amd64"
GOHOSTOS="darwin"
GOOS="darwin"
GOPATH=""
GORACE=""
GOROOT="/usr/local/Cellar/go/1.6.2/libexec"
GOTOOLDIR="/usr/local/Cellar/go/1.6.2/libexec/pkg/tool/darwin_amd64"
GO15VENDOREXPERIMENT="1"
CC="clang"
GOGCCFLAGS="-fPIC -m64 -pthread -fno-caret-diagnostics -Qunused-arguments -fmessage-length=0 -fno-common"
CXX="clang++"
CGO_ENABLED="1"
```
需要设置的环境变量包括:GOPATH ,GOBIN 以及把GOBIN加入到PATH中,GOROOT变量默认已经设置好。
配置bash_profile

```
#go
export GOPATH=~/work/workspace/gopath/
export GOBIN=$GOPATH/bin
export PATH=$PATH:$GOBIN
```
保存之后，使立即生效 `source .bash_profile`

## 升级Golang

```
➜  ~ brew upgrade  go

➜  ~ go version
go version go1.10 darwin/amd64

```

## 问题

### 安装mysql驱动异常

```
➜  go1test go get -u github.com/go-sql-driver/mysql
# github.com/go-sql-driver/mysql
../gopath/src/github.com/go-sql-driver/mysql/utils.go:81: undefined: cloneTLSConfig

➜  ~ go version
go version go1.6.2 darwin/amd64

```
解决方法：升级go


