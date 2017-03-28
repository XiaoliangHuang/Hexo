---
title: Git版本控制
date: 2017-03-23 16:47:23
tags: [Git, 技术]
categories: 技术
---

# Git 概览 #

下面这张图是根据自己理解画出来的关于git的一个大概的知识框架图，分别从版本控制的发展,Git的优点,原理和使用的角度队涉及到的知识进行了一个梳理, 最后还有一些遇到过的小问题或者是Tip的总结.

![](git_knowledge_map.svg)


## 版本控制的发展和历史 ##

虽然只有几十年的时间, 但是版本控制已经有着几代的更新,最近几年,Git的发展则出现了一统江湖的局面。仔细想想，除了Git本身的优点之外，存储空间和网络速度的发展，也使得snapshot这种全量模式对空间/网络传输需求大的问题变得不那么严重，不然Git也不会有今天的江湖地位了。

下面这张图描述了版本控制系统的一个总体概况，里面的绝大多数内容都来自[版本控制的前世和今生](http://gotgit.readthedocs.io/en/latest/01-meet-git/010-scm-history.html#cvs)，有兴趣的同学可以去查看原文。

![](version_control.svg)


# Git的原理和实现机制 #

## 分布式的版本控制系统 ##

相比于TFS/Subversion/SD这些集中式的版本控制系统，Git采取的是一种分布式的结构, 一个本地的repository就是一个完整的单机版本控制系统,加上服务器上面的remote repository以后,不同主机/用户之间就可以进行分享和协作了。

## 保存修改文件的 snapshot 而不是增量 ##

和以前集中式的版本控制系统（SVN...）所不同的是，当某个文件发生修改的时候，Git所保存的是一个全新的备份。如下图所示：

<div align="center">
<img src="git_snapshot.png" width="80%" align="center">
</div>

[//]:![](git_snapshot.png)

这样带来的好处是切换速度快，要查看某个版本（某个commit）的project信息的时候，只需要把对应的文件拿出来就可以，所以Git从任意一个版本都能够很方便的进行build或者是拆分出新的branch出来。

而不好的地方就是每次一点点小小的修改都要保存一整个文件的snapshot，这样空间上会非常浪费，从而影响push和get的性能，但是因为Git本身是分布式的，常规的操作都是在本地的repository里面进行的，也就是硬盘读写，加上网络速度和存储空间越来越好，这部分的影响相对会减轻。


## 工作区， 暂存区 和 Git仓库 ##

下图给出了Git 版本控制系统的基本机构：

<div align="center">
<img src="git_structure.png" width="70%" align="center">
</div>

[//]:![](git_structure.png)

它包含四个主要部分：
```
1. WorkSpace: 
    也叫Working Area或者工作目录，对项目的某个版本独立提取出来的内容。 这些从 Git 仓库的压缩数据库中提取出来的文件，放在磁盘上供你使用或修改。
2. StagingArea: 
    也叫做Index或者暂存区域，是一个文件，保存了下次将提交的文件列表信息，一般在 Git 仓库目录中。
3. Repository: 
    也叫Git仓库，是 Git 用来保存项目的元数据和对象数据库的地方。 这是 Git 中最重要的部分，从其它计算机克隆仓库时，拷贝的就是这里的数据。
4. RemoteRepository: 
    网络服务器上面Git Center里面，用来保存Repository的地方，跟本地Repository相对应，通过Pull/Push/Clone等方式进行操作。
```

这四个部分，尤其是前面local的三个部分，构成了我们日常使用Git的主要场景，在尝试过一些git的基本操作，比如下面这些命令以后

```
$ git pull
$ git status
$ git add .
$ git commit -m "xxx"
$ git push
```
需要再进一步的时候，就需要对于这样一个基本结构，还有这些地方与Git管理的文件的状态之间的对应关系有着清晰的认识。

## Git文件的状态和迁移 ##
对应于上面一节提到的Git系统的几个部分，在某个时刻，使用Git管理的文件就有可能处于不同的位置，拥有不同的状态。下面的图就给出了Git文件可能的状态

<div align="center">
<img src="git_file_states.png" width="80%" align="center">
</div>

[//]:![](git_file_states.png)

使用
```
$ git status -s
```
命令就可以查看新增/修改的文件和状态:
<div align="center">
<img src="git_status_bash_2.png" width="70%" align="center">
</div>

[//]:![](git_status_bash_2.png)

还有一些UI的tool,比如VS Code,会有这更好的状态表示：

<div align="center">
<img src="git_status_vscode.png" width="70%" align="center">
</div>

[//]:![](git_status_vscode.png)


但是使用命令行仍然是Git最主要和最为方便的模式,因此当你使用了一段时间的Git以后，使用 *git status -s* 命令查看文件的状态，并且清晰的知道其所在的位置（工作区，暂存区和git repository），和可能的状态转移，以及使用什么命令进行操作，就非常的重要了。这也是区别知道git和真正会用git的很重要一点。

## Rollback: Clean，Reset, Checkout, Revert的使用 ##

最初的时候，我们的在git里面对文件的操作都是正向的，也就是 add/commit/push 这些命令，如果有问题，也可以通过修改以后再次提交的方法进行覆盖。但是在管理很多的code的时候，这样人肉的方法就会显得效率很低。

作为版本控制的系统，Git其实已经提供了丰富的命令来支持rollback的操作， 有
```
$ git clean
$ git reset
$ git checkout
$ git revert
```

### 作用域 ###

Reset/Checkout 命令可以支持在文件level和commit的level进行操作，参数当中是否包含文件路径决定了这些操作是对文件还是对commit有效。

### 主要 Scenarios ###

下面以基本scenario为例，来看看reset操作怎么满足我们rollback的需求。

前面提到过，Git 主要有三个部分组成（不考虑 remote repository）： Workspace，Staging Area 和 Repository。当文件处于他们当中不同的位置（对应不同的状态）的时候，通过对于的操作都可以进行rollback， 以文件123.txt 为例：

#### Rollback from Create (Not yet Add) ####

   这个时候，文件的状态是 unstaged/untracked， 只是在本地有一个文件而已，要进行删除的话，使用 git clean 命令。
   ```
   $ git clean -f 
   ```
   主要的参数有 d/n/f：更详细的内容可以参考 [git clean --help]()
```
    -d: 同时删除untracked的目录
    -n: 显示将会做什么，但是不会真的删除文件
    -f：force， 最终删除文件
```
下图显示了其状态的变化


#### Rollback from Add ####

如果文件已经使用 git add 


#### Rollback from Commit ####












[//]:![](rollback_redundance.png)>
<div align="center">
<img src="rollback_redundance.png" width="70%" align="center">
</div>


## Git的内部实现 ##

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

[图解git -- 用图片分析学习git原理](http://huanglei.me/git-theory.html)