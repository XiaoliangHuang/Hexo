---
title: Git版本控制
date: 2017-03-23 16:47:23
tags: [Git, 技术]
categories: 技术
---


# Git的原理和实现机制 #
## Git 文件的状态机 ##

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


## Paging 分页器 ##

这部分内容摘自 [爱上Git的理由](http://gotgit.readthedocs.io/en/latest/01-meet-git/020-love-git.html#git)

虽然拥有图形化的客户端，但Git更有效率的操作方式还是命令行操作。使用命令行操作的好处一个是快，另外一个就是防止鼠标手的出现。Git的命令行进行了大量的人性化设计，包括命令补全、彩色字符输出等，不过最具特色的还是无处不在的分页器。

在操作其他版本控制系统的命令行时，如果命令的输出超过了一屏，为了能够逐屏显示，需要在命令的后面加上一个管道符号将输出交给一个分页器。例如：
```
$ svn log | less
```
而Git则不用如此麻烦，因为常用的Git的命令都带有一个分页器，当一屏显示不下时启动分页器。分页器默认使用less命令（less -FRSX）进行分页。

因为less分页器在翻屏时使用了vi风格的热键，如果您不熟悉vi的话，可能会遇到麻烦。下面是在分页器中常用的热键：
```
- q：退出分页器。
- h：显示分页器帮助。
- 空格: 下翻一页
- b: 上翻一页。
- d/u：分别代表向下翻动半页和向上翻动半页。
- j/k：分别代表向上翻一行和向下翻一行。
- 如果行太长被截断，可以用左箭头和右箭头使得窗口内容左右滚动。
- 输入/pattern：向下寻找和pattern匹配的内容。
- 输入?pattern：向上寻找和pattern匹配的内容。
- 字母n或N：代表向前或向后继续寻找。
- 字母g：跳到第一行；字母G：跳到最后一行；输入数字再加字母g：则跳转到对应的行。
- 输入!<command>：可以执行Shell命令。
```
对于默认未提供分页器的Git命令，例如git status命令，可以通过下面任一方法启用分页器：

在git和子命令（如status）之间插入参数-p或--paginate，为命令启用内建分页器。如：
```
$ git -p status
```
设置Git配置变量，设置完毕后运行相应的命令，将启用内建分页器。
```
$ git config --global pager.status true
```
Git命令的分页器支持带颜色的字符输出，对于太长的行则采用截断方式处理（可用左右方向键滚动）。如果不习惯分页器的长行截断模式而希望采用自动折行模式，可以通过下面任一方法进行设置：

通过设置LESS环境变量来实现。
```
$ export LESS=FRX
```
或者通过定义Git配置变量来改变分页器的默认行为。
```
$ git config --global core.pager 'less -+$LESS -FRX'
```
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

##  Git 在 windows 下中文乱码 ##

在windows下面使用git来添加中文名称的文件名的时候，会遇到类似于下面的，文件名被转码，无法识别的问题：
```
  \316\304\261\276\316\304\265\265.md
```

其原因是 Git 使用的是 utf-8 的编码方式，而在windows下面默认采用的则是Unicode，因而出现了编码和解码不一致的问题。解决的方法是在git上面进行下面的设置, 让git支持utf-8编码 ( [解决 Git 在 windows 下中文乱码的问题](https://gist.github.com/nightire/5069597) ) ：
```
$ git config --global core.quotepath false  		# 显示 status 编码
$ git config --global gui.encoding utf-8			# 图形界面编码
$ git config --global i18n.commit.encoding utf-8	# 提交信息编码
$ git config --global i18n.logoutputencoding utf-8	# 输出 log 编码
$ export LESSCHARSET=utf-8
# 最后一条命令是因为 git log 默认使用 less 分页，所以需要 bash 对 less 命令进行 utf-8 编码
```





# Rerence #
[版本控制的前世和今生](http://gotgit.readthedocs.io/en/latest/01-meet-git/010-scm-history.html#cvs)

[为什么选择 Git](https://www.git-tower.com/learn/git/ebook/cn/command-line/appendix/why-git)

[爱上Git的理由](http://gotgit.readthedocs.io/en/latest/01-meet-git/020-love-git.html#git)

[Git 权限控制](http://www.cnblog.me/2016/04/04/git-permissions/)

[Git Config 命令查看配置文件](https://cnbin.github.io/blog/2015/06/19/git-config-ming-ling-cha-kan-pei-zhi-wen-jian/)

[如何使用 Git Submodule](http://linlexus.com/git-submodule-usage/)

[解决 Git 在 windows 下中文乱码的问题](https://gist.github.com/nightire/5069597)

[Learning Git Internals by Example](http://teohm.com/blog/learning-git-internals-by-example/)

[通过示例学习Git内部构造（译）](http://blog.xiayf.cn/2013/09/28/learning-git-internals-by-example/)