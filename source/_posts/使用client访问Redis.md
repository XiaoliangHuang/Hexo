---
title: 使用 VS Code + Python Client 访问 Redis
date: 2017-04-18 21:13:54
tags: [Redis, VS Code, Python]
categories: [技术]
---

作为一种缓存工具, Redis 肯定需要结合我们的产品进行使用，而不是停留在`Command line` 和 `Redis Desktop Manager` 这样的 ‘练手’ 阶段，下面我们就以`Python Client`为例说说Redis编程中的使用。

我们知道，Redis是一种 `Client + Server`的模式，所以在实际使用当中，也是分两个部分


>__* Server:__
>自己搭建Redis的server, 建立内存数据库，需要的时候，通过RDB/AOF进行持久化,并且能够通过集群的方式进行扩展。
>
>__* Client__
>某种语言的扩展支持，通过package之类的模式import，对服务器进行访问和操作。

Server和Clients的关系如下图所示：

![](redis_in_windows.svg)

# Redis Clients 支持的语言 #

打开Redis的官方网站[Clients的列表](https://redis.io/clients)，有差不多50种，可以说，Redis几乎支持所有常用的编程语言，这也说明了Redis在如今有多么受欢迎。

![](redis_clients_list.png)

# Windows 下的环境配置#

要问我为什么是windows下的而不是更常用的环境，我只能说，这是作为小软的码农的一点狭隘的情怀了

## Python 安装 ##
Windows下面安装Python很简单， 去官网下载最新的安装文件，一路next基本就好了，需要注意的选项是：
> 把python加入到环境变量
> 安装Pip,也就是python的package管理工具，如果vs的nuget，node.js里的npm一样

## 通过pip安装Redis package ##

Redis 在每种语言里都有好几种的client可选，拿Python来说，就有Pyredis, redis-py 等十多种，这里选取官方推荐的 redis-py。

![](redis-py.png) 
 
进入[redis-py 官方github页面](https://github.com/andymccurdy/redis-py)，能看到使用说明。其实也就是一个命令 


> $ pip install redis


通过这个命令，就能在本地安装一个python的Redis package了(使用 *Administrator*权限)。

![](pip_install_redis.png)

安装好以后，进入python的环境import redis就可以访问redis server进行测试了: 

```python
C:\windows\system32>python
>>> import redis
>>> cli = redis.StrictRedis(host='localhost', port = 6001)
>>> cli.set('client name', 'command line')
True
>>> cli.get('client name')
b'command line'

```

# 使用 VS Code 编辑 Python #
Python 通常是当作一种脚本语言来使用的，是直接被解释使用，而不是被编译的。所以严格意义上讲，Python的IDE应该只是一个调试运行的编辑器。

网上有很多Python的IDE，比如说著名的Pycharm，但是自从使用了VS Code以后，它就一下子占据了exclusive的位置。 下面说说怎么在VS Code 里面配置使用Python。

## 安装 VS Code ##
去[VS Code 官网](https://code.visualstudio.com/)下载安装最新版本就可以了。


## Python 扩展插件 ##

VS Code 对于python的支持是通过 Python的扩展插件来实现的。在extensions里面搜索`Python`，就可以得到很多的Python插件，像`Python`, `Python for VSCode`, `MagicPython`都可以支持Python, 并且提供语法高亮，引用提示等等功能。

![](vscode_python_extension.png)

安装好以后reload一下，VS Code 里面就可以编辑/运行/调试 Python脚本了。

## Linting  ##

Linting 是VS Code里面用来分析代码，帮你找出来潜在Warnings/Errors的插件。
>Tip: [Don Jayamanne's Python extension](https://marketplace.visualstudio.com/items?itemName=donjayamanne.python) gives you the option of using three different linters - [Pylint](https://www.pylint.org/), [Pep8](https://pypi.python.org/pypi/pep8), and [Flake8](https://flake8.readthedocs.io/en/latest/). See the [wiki](https://github.com/DonJayamanne/pythonVSCode/wiki/Linting) for more details.



## 在 VS Code 里面编辑 Python 脚本##

VS Code 默认情况下只是一个编辑器，如果你想做的只是再一个比较好用的编辑器里面写Python的Script，那打开VS Code 的窗口，然后new 一个新的.py文件，然后进行编辑就可以了，由于安装了extension，VS Code 可以提供很好的语法提示：

![](python_editor.png)


## 通过 VS Code 运行 Python 脚本 ##

但是当我们需要进一步在VS Code里面直接运行/Debug Python的脚本, 而不需要重新开启一个命令行去运行`python xxx.py`的时候，就需要用到VS Code 里面的 `Task`, 它的作用就像是 VS 里面的Solution，通过config文件来指定运行脚本/代码所需要的命令参数等等。

具体步骤是：

1. Open the **Command Palette** (`Ctrl+Shift+P`).
2. Select the **Tasks: Configure Task Runner** command and you will see a list of task runner templates.
3. Select **Others** to create a task which runs an external command.
4. Change the command to the command line expression you use to build your application (e.g. g++ -g main.cpp).
5. Add any required args (e.g. -g to build for debugging).
6. You can now build your application with (`Ctrl+Shift+B`)

然后就会看到，在你的目录下面，会新生成一个`.vscode`的子目录，包含了一个`task.json`文件，对于运行python来说，其内容可以配置成如下的参数，让它运行当前打开的文件：

![](task_json.png)

关于task的设置可以参考[官方文档](https://code.visualstudio.com/docs/editor/tasks)。一般情况，`.vscode`目录下面会包含三个config文件：

![](vscode_config.png)

其中`tasks.json`就是配置运行环境的，`settings.json`下的配置能够覆盖VS Code的默认配置，`launch.json`配置调试环境

## Debug ##

经过上面的步骤，我们已经可以运行Python Scripts了，如果要进行debug的话，可以`F5`或者从菜单选择 Debug -> Start Debugging。

但是这时候我们并没有配置`launch.json`，会提示 *Error: Failed to launch the Python Process, please validate the path 'python'*

![](debug_error.png)

选择`Open launch.json` 就会添加一个Python的config文件，然后就能debug了，但是有可能中间需要重启一下VS Code才能生效。

![](debug.png)

# Reference #
[Python on Visual Studio Code](https://code.visualstudio.com/docs/languages/python)
[Integrate with External Tools via Tasks](https://code.visualstudio.com/docs/editor/tasks)
[Redis 命令参考](http://redisdoc.com/)
[VS Code搭建Python开发环境](https://xin053.github.io/2016/06/11/VS%20Code%E6%90%AD%E5%BB%BAPython%E5%BC%80%E5%8F%91%E7%8E%AF%E5%A2%83/)