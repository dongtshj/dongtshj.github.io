---
title: 一台电脑同时使用两个版本的python
categories: Python
date: 2020-01-14
---

可以更改`python`解释器的文件名以区分两个不同版本的`python`，但最好不要这样做。

### 官方推荐做法
---

> 我们在安装`Python3（>=3.3）`时，`Python`的安装包实际上在系统中安装了一个启动器`py.exe`，默认放置在文件夹`C:\Windows\`下面。这个启动器允许我们指定使用`Python2`还是`Python3`来运行代码，当然前提是你已经成功安装了`Python2`和`Python3`

如果你有一个`Python`文件叫`hello.py`，那么你可以这样用`Python2`运行它：

`py -2 hello.py`

类似的，如果你想用`Python3`运行它，就这样：

`py -3 hello.py`

#### 去掉`-2/-3`参数

> 每次运行都要加入参数`-2/-3`还是比较麻烦，所以`py.exe`这个启动器允许你在代码中加入说明，表明这个文件应该是由`python2`解释运行，还是由`python3`解释运行。说明的方法是在代码文件的最开始加入一行。

`#! python2`

或者

`#! python3`

分别表示该代码文件使用`Python2`或者`Python3`解释运行。这样，运行的时候你的命令就可以简化为：

`py hello.py`

#### #! python2/#! python2 和 # coding: utf-8 哪个写在前面？

编码放到后面，像下面这样：
```
#! python2
# coding: utf-8
```

#### 使用pip
>当`Python2`和`Python3`同时存在于`windows`上时，它们对应的`pip`都叫`pip.exe`，所以不能够直接使用 `pip install` 命令来安装软件包。而是要使用启动器`py.exe`来指定`pip`的版本。命令如下：

`py -2 -m pip install XXXX`

或者

`py -3 -m pip install XXXX`

### 参考：
* https://www.zhihu.com/question/21653286