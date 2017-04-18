---
title: VS Code + Python Client 使用 Redis
date: 2017-04-18 21:13:54
tags: [Redis, VS Code, Python]
categories: [技术]
---

在使用过了`command line` 和 `Redis Desktop Manager` 这样的GUI以后，下面我们以`Python`为例子看看Redis client的使用。

使用某种语言的client最大的好处，当然是可以讲Redis的操作和其他的处理合并在一起，这也是实际应用当中最常用的方式。

# Redis Clients 支持的语言 #

打开Redis的官方网站[Clients的列表](https://redis.io/clients)，有差不多50种，可以说，Redis几乎支持所有常用的编程语言，这也说明了Redis在如今有多么受欢迎。

# Windows 下的环境配置#

不要问我为什么是windows下的，作为小软的码农，这点狭隘的情怀还是要有的

## Python ##
Windows下面安装Python很简单， 去官网下载最新的安装文件，一路next基本就好了，需要注意的选项是：
> 把python加入到环境变量
> 安装Pip,也就是python的package管理工具，如果vs的nuget，node.js里的npm一样


