---
title: Git版本控制
date: 2017-03-23 16:47:23
tags:
categories:
---


# Tips #
## Config Git--局部设置与全局设置 ##

最开始使用 git 的时候，都是按照网上的介绍 step by step，然后切到一个新的地方，就会发现一些奇怪的事情，比如说，我安装了Git的时候，是用来sync BitBucket上面的Repository的，但是后来用来处理Github上面的Repository为什么记录里面显示的还是之前的BitBucket账号提交的呢？

原因就是Git的的设置方式。有全局设置和局部设置

### 局部设置 ###
当你Init或者Clone一个Git Repository的时候，Git都会在指定目录（一般是当前目录）建立一个.git的文件夹，用来保存当前Repository的相关内容，跟这个Repository相关的设置也在这个目录当中，保存在一个名为config的文件当中
```
--.git
  |
  --config
```
其中的内容是诸如下面的config
```
[remote "Hexo"]
	url = https://github.com/laomanco/Hexo.git
	fetch = +refs/heads/*:refs/remotes/Hexo/*
[branch "master"]
	remote = Hexo
	merge = refs/heads/master
[user]
    name = laomanco
    email =laomanco@gmail.com
```
除了直接修改config文件，还可以通过命令行来进行设置，例如，要设置当前Git Repository Commit 等等的用户信息

```
$ git config user.name laomanco
$ git config user.email laomanco@gmail.com
```
要查看config的内容的话，用get命令就可以，更多具体内容可以参考[Git Config 命令查看配置文件](https://cnbin.github.io/blog/2015/06/19/git-config-ming-ling-cha-kan-pei-zhi-wen-jian/)
```
$ git config --get user.name
laomanco
```

### 全局设置 ###

在安装Git的时候，系统就会为当前用户添加一个全局的系统设置，存放在下面的目录当中

```
C:\Users\<username>\.gitconfig
```
里面包含的config内容格式与局部config非常的类型，也可以通过修改文件和命令行的方式进行修改，不过通过命令行进行设置的时候，需要在config命令里面加上--global的参数

```
$ git config --global user.name laomanco
$ git config --global user.email laomanco@gmail.com
$ git config --global --get user.name
```

# Git 文件的状态机 #

最开始用Git 进行文件管理的时候，基本上就是几个最基本的命令：

```
$ git pull
$ git status
$ git add .
$ git commit -m "xxx"
$ git push
```

![](git_state_machine.png)
![](git_state_machine2.png)










# Rerence #
[版本控制的前世和今生](http://gotgit.readthedocs.io/en/latest/01-meet-git/010-scm-history.html#cvs)

[为什么选择 Git](https://www.git-tower.com/learn/git/ebook/cn/command-line/appendix/why-git)

[爱上Git的理由](http://gotgit.readthedocs.io/en/latest/01-meet-git/020-love-git.html#git)

[Git 权限控制](http://www.cnblog.me/2016/04/04/git-permissions/)

[Git Config 命令查看配置文件](https://cnbin.github.io/blog/2015/06/19/git-config-ming-ling-cha-kan-pei-zhi-wen-jian/)

[如何使用 Git Submodule](http://linlexus.com/git-submodule-usage/)
