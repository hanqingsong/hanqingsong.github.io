---
title: python安装使用
date: 2018-05-14 22:32:24
categories:
    - python
tags:
    - python
---
## python安装
```
brew search python
brew install python3
```

## Setuptools & Pip
setuptools 和 pip 是最重要的两个Python第三方软件包。一旦安装了它们，就可以通过一条指令下载、安装和卸载可获取到的 Python应用包，还可以轻松地将这种网络安装的方式加入到自己开发的Python应用中。
Python 2.7.9 以及之后版本(Python2 系列)，和Python 3.4以及之后版本均默认包含pip。
运行以下命令行代码检查pip是否已经安装：
```
$ command -v pip
```
pip 用于Python 2的，而 pip3 用于Python 3。
```
$ command -v pip3
```

安装方法
```
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
python get-pip.py
```


## Pipenv安装
Pipenv 是一个 Python 项目依赖管理工具，如果你熟悉 Node.js 的 npm 或者 Ruby 的 bundler ，他们是和 Pipenv 非常相似的工具。尽管 pip 也可以安装 Python 的包，但是 Pipenv 被推荐的理由是因为 Pipenv 为使用者提供了更加方便的依赖管理。
虚拟环境是保持项目依赖独立的一种方式，Pipenv 是实现这种方式的其中一个工具，同时 Pipenv 也是一个依赖管理工具，也就是说 Pipenv 集成了 pip 和 virtual environment 的功能，通过创建虚拟环境，可以保证项目之间的的依赖互不干扰。

pip 安装 Pipenv ：
```
$ pip install --user pipenv
```
通过运行 python -m site --user-base 找到 用户基础目录，然后把 bin 加到目录末尾。通过 修改 ~/.profile 永久地设置 PATH。

## virtualenv
virtualenv 是一个创建独立的 Python 环境。 virtualenv 会创建一个文件夹，其中包含使用 Python 项目所有所需的可执行文件。
它可以单独使用，用于代替 Pipenv 。
pip 安装 virtualenv ：
```
$ pip3 install virtualenv
$ virtualenv --version
```

1. 为项目创建一个虚拟环境：
```
$ cd my_project_folder
$ virtualenv my_project
```
virtualenv my_project 将会在当前目录创建一个文件夹来存放 Python 的可执行文件以及拷贝一份 pip 库，这样就能安装其他包了。
2. 开始使用虚拟环境前，需要先激活：
```
$ source my_project/bin/activate
```
你所添加的扩展和运行的环境， 都处于这个隔离环境中了， 如果还需要一个python3.5的环境， 你可以在安装好python3.5（不要在任何隔离环境中）后， 创建隔离环境时，指定python版本即可
```
virtualenv -p python3.5 py3.5
virtualenv -p /usr/bin/python2.7 my_project
```

3. 如果你在虚拟环境中暂时完成了工作，可以这样停用它：
```
$ deactivate
```

## virtualenvwrapper
virtualenvwrapper 是virtualenv的一些列扩展，它提供了诸如 mkvirtualenv, lssitepackages 等命令行工具，特别是 workon 命令行工具，当你需要使用多个virtualenv目录时使用该工具特别方便。
它把您所有的虚拟环境都放在一个地方。
```
$ pip install virtualenvwrapper
$ export WORKON_HOME=~/Envs
$ source /usr/local/bin/virtualenvwrapper.sh
```

## virtualenv-burrito
使用 virtualenv-burrito ，你可以只要使用一条命令就将 virtualenv + virtualenvwrapper 环境搭建起来。

## autoenv
当您 cd 进入一个包含 .env 的目录中，就会 autoenv 自动激活那个环境。
```
$ brew install autoenv
```

## 资料
http://pythonguidecn.readthedocs.io/zh/latest/dev/virtualenvs.html


