---
title: python中venv，pyvenv，pyenv，virtualenv，virtualenvwrapper，pipenv等有什么区别
categories:
  - python
tags:
  - python
date: 2018-05-22 21:50:45
---


## PyPI软件包不在标准库中：

* virtualenv是一个非常受欢迎的工具，为Python库创建了孤立的Python环境。 如果您不熟悉此工具，我强烈建议您学习它，因为它是非常有用的工具，我将在此答案的其余部分进行比较。

它通过在目录（例如： env/ ）中安装一堆文件，然后修改PATH环境变量以将其前缀到自定义bin目录（例如： env/bin/ ）。 python或python3二进制文件的完整副本放置在此目录中，但Python被编程为首先在环境目录中查找相对于其路径的库。 它不是Python标准库的一部分，但是由PyPA（Python Packaging Authority）正式获得祝福。 激活后，您可以使用pip在虚拟环境中安装软件包。

* pyenv用于隔离Python版本。 例如，您可能希望根据Python 2.6,2.7,3.3,3.4和3.5测试代码，因此您需要一种在它们之间切换的方法。 一旦被激活，它将在PATH环境变量~/.pyenv/shims加上~/.pyenv/shims ，其中有与Python命令（ python ， pip ）匹配的特殊文件。 这些不是Python发出的命令的副本; 它们是基于PYENV_VERSION环境变量或.python-version文件或~/.pyenv/version文件运行的Python版本的特殊脚本。 pyenv也使得使用命令pyenv install更容易地下载和安装多个Python版本的过程。

* pyenv-virtualenv是与pyenv相同的作者pyenv的插件，可以方便地同时使用pyenv和virtualenv 。 但是，如果您使用的是Python 3.3或更高版本， pyenv-virtualenv将尝试运行python -m venv如果可用），而不是virtualenv 。 如果您不想要方便的功能，您可以在不使用pyenv-virtualenv情况下使用virtualenv和pyenv 。

* virtualenvwrapper是一组对virtualenv的扩展（请参阅docs ）。 它给你的命令像mkvirtualenv ， lssitepackages ，特别是在不同的virtualenv目录之间切换的工作。 如果您想要多个virtualenv目录，此工具特别有用。

* pyenv-virtualenvwrapper是与pyenv相同的作者的pyenv ，方便地将virtualenvwrapper集成到pyenv 。

* pipenv ，Kenneth Reitz（ requests的作者），是一个全新的（可能是实验性的）项目，旨在将Pipfile，pip和virtualenv组合成一个命令行命令。

## 标准库：
* pyvenv是Python 3附带的一个脚本，但是在Python 3.6中已经弃用，因为它有问题（更不用说混淆的名字）。 在Python 3.6+中，完全相同的是python3 -m venv 。

* venv是Python 3附带的软件包，您可以使用python3 -m venv运行（尽管某些原因有些发行版将其分离成独立的发行版包，如Ubuntu / Debian中的python3-venv ）。 它为virtualenv提供了类似的目的，并以非常类似的方式工作，但它不需要复制Python二进制文件（Windows除外）。 如果您不需要支持Python 2，请使用此功能。在撰写本文时，Python社区似乎对virtualenv感到满意，我没有听说过venv 。
