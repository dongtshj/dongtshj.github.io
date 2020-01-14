---
title: 软件包管理系统：pip
categories: Python
date: 2020-01-14
---

### 维基百科对`pip`的介绍：
---

>   `pip`是一个以`Python`计算机程序语言写成的软件包管理系统，他可以安装和管理软件包，另外不少的软件包也可以在“`Python`软件包索引”（英语：`Python Package Index`，简称`PyPI`）中找到。

> `pip`的其中一个主要特点就是其方便使用的命令行接口，这让用户可以透过一句文字命令来轻易地安装`Python`软件包：

* 安装软件命令：`pip install some-package-name`

> 此外，用户也可以轻易地透过命令行来移除软件包：

* 卸载软件命令：`pip uninstall some-package-name`

> pip也拥有一个透过“需求”文件来管理软件包和其相应版本数目的完整列表之功能，这容许一个完整软件包组合可以在另一个环境（如另一部电脑）或虚拟化环境中进行有效率的重新创造。这个功能可以透过一个已正确进行格式化的文本文件和以下的命令来完成：

* 安装或更新已经设定好的这个软件列表：`pip install -r requirements.txt`

### 补充
---

补充一个**更新软件**的命令：

* 更新软件命令：`pip install -U some-package-name`

更新`pip`它自身使用下面命令：

* 更新`pip`自身：`python -m pip install --upgrade pip`

另外，**降级软件版本**使用：
* 降级软件版本：`pip install some-package-name==3.8.1`

**最后，和`python`的`pip`对应的，有个叫做`npm`（全称 `Node Package Manager`，即“`node`包管理器”）的软件包管理系统，是`Node.js`默认的、以`JavaScript`编写的软件包管理系统。**




