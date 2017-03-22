---
title: 使用Hexo搭建博客
date: 2017-03-21 00:51:38
tags: [Hexo, 博客]
categories: 技术
comments: true
---

最近在整理笔记的时候，发现OneNote跟EverNote这样的工具都不够方法，主要是排版，引用，语法强调跟层次结构上面很难满足需求，后来又看了简书之类的平台，本来是为了找一个写技术文档的地方的，最后发现了用Hexo来建立自己的博客的方式，在满足自己需求的前提下，还能够拥有自己的专属域名，觉得很cool，就研究了一下。


Hexo是一款基于Node.js的静态博客框架，所谓的静态博客，就是说文章是build成一个个的静态页面，而不是存在数据库里面动态加载。

![](hexo_xmind.png)

上面这张图给出了利用Hexo搭建自己的博客所涉及到的几个方面, 归纳起来就是下面几个问题：


>(1). 如何搭建博客？
>(2). Host在哪里？
>(3). 怎么发布？
>(4). 域名管理
>(5). 代码管理 


# 如何搭建网站：使用Hexo #

使用Hexo来搭建一个博客系统，包含下面几个部分的工作：


## Hexo环境配置 ##

[Hexo](http://hexo.io/zh-cn/) 是基于Node.js的，安装好Node以后，直接通过npm就可以安装Hexo以及相关的扩展。然后因为是Hexo是通过Git来发布信息的，所以Git，以及Github（或者其他的替代产品，e.g.国内的BitBucket，CitCafe）的账号也是必须的。

准备工作：
```
(1). 安装Node，并通过npm安装Hexo
(2). 安装Git并且注册GitHub账号 
```
*Note: 直到后面真正发布网站之前，Git其实是并不需要的，为了不引起一些不必要的疑惑，这里讲跳过Git相关的具体内容。*

Node 下载并安装好了以后，在命令行输入
```
$ npm --version

```
测试一下 npm 是不是装好了（npm 是node 用来管理各种package的工具，类似于Visual Studio里面的nuget，关于npm包管理工具的详细信息，可以参考[npm 模块安装机制简介](http://www.ruanyifeng.com/blog/2016/01/npm-install.html)）。

一切OK以后，然后就可以安装Hexo了。真正安装的时候，比想象中要简单得多的多，命令行里面输入下面的命令就搞定了：
```
$ npm install -g hexo-cli
```
安装好以后，输入
```
$ hexo
```
看是否出现Commands的提示，有就说明一切正常，可以进行下一步了

## 新建一个Blog网站 ##

安装 Hexo 完成后，请执行下列命令，Hexo 将会在指定文件夹中新建所需要的文件。

```
$ hexo init <folder>
$ cd <folder>
$ npm install
```
经过这三个简单的命令，一个Hex的博客网站就建成了。默认情况下，会安装一个landscape的主题，和一篇hello world的文章。

## 进行本地测试 ##
在命令行里面输入
```
$ hexo s
```
就可以启动一个本地的service
```
$ hexo s
INFO  Start processing
INFO  Hexo is running at http://localhost:4000/. Press Ctrl+C to stop.

```
在浏览器输入[localhost:4000](http://localhost:4000/) (4000是默认端口)就可以看到下面的网站首页了

<div align="center">
<img src="hello_world.png" width="80%" align="center">
</div>

完成以后，会生成一个Hexo项目的文件夹，名字就是前面 *init* 命令参数里面的*folder*，各个文件夹的含义参考[官方网站说明](https://hexo.io/zh-cn/docs/setup.html)

## 往Blog里面添加一篇新的文章##

<div align="center">
<img src="vs_code.png" width="80%" align="center">
</div>

## 扩展功能：设置Blog的主题Theme ##
## 扩展功能: Catalog/Tag/Comments... ##



# Host在哪里：Cloud 或者Git Pages #

## Git Pages ##

## Git ##

如何注册GitHub以及建立Repository,可以参考 [20分钟教你使用hexo搭建github博客](http://www.jianshu.com/p/e99ed60390a8),。

在最开是的需求里面，Git 是用来将生成的静态网站内容同步到GitHib上去的, 所以这里需要注册GitHub账号并且下载客户端, 实际上, 我们最会用到两个Git Repository, 一个用来上传Build出来的静态网站, 另外一个这是用来管理我们自己整个Hexo的project, 以便于我们可以随时的在不同的电脑上面进行修改. 其中的关系可以用下面的图来表示：

[//]:![](git_repo.png)>
<div align="center">
<img src="git_repo.png" width="80%" align="center">
</div>

需要注意的是，在create repository的时候，名字必须是{username}.github.io, 也就是GitHub默认分配给你账号的域名。详细原因在后面讲Deploy的章节来解释


### 第三方Cloud Service ###


## 3. 如何发布：deploy到git repository ##


## 4. 域名管理 ##

### 购买域名 ###
### 解析绑定域名到自己的Service ###

## 5. 代码管理 ##

### 新建一个Git Repository或者 Branch ###
### Project 结构： 那些需要同步 ###

## 6. Refrence ##
[npm 模块安装机制简介](http://www.ruanyifeng.com/blog/2016/01/npm-install.html)
[Hexo官方网站](https://hexo.io/zh-cn/)
[Markdown 中文文档](https://markdown-zh.readthedocs.io/en/latest/blockelements/)
[Xmind 官方网站](http://www.xmind.net/)
[让思路更清晰——我是怎样使用思维导图的](https://blog.coding.net/blog/coding-mindmap)
[iPic + Typora，方便快捷地在 Markdown 中插图](https://sspai.com/post/36275)
[如何在 Markdown 注释一段文字](http://www.jianshu.com/p/9be87e7e15bf)
[MarkDown使用小技巧](http://www.jianshu.com/p/9d94660a96f1)
[MarkDown编辑器 Typora](https://typora.io/)

