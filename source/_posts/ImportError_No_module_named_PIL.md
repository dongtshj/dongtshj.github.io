---
title: ImportError No module named PIL
categories: Python
date: 2020-01-01
---

今天在使用github上的开源项目，抢12306的春运火车票。

跑python脚本的时候，发现本地机器缺少了一些必需的模块。

大部分类似下面这样的错误提示：

`ImportError No module named module_name`

都可以使用下面的命令，安装模块后解决：

`pip install module_name`

但是，唯独下面这个报错：

`ImportError No module named PIL`

不能使用对应的，下面这个命令解决：

`pip install module_name`

网上搜了一圈，正确的安装命令如下：

`pip install Pillow`

`PIL`应该是python的一个图像处理库，这里估计是用来识别12306的验证码的。但是，貌似这个已经被弃用了，更推荐`Pillow`这个库，`Pillow`应该是`PIL`项目的一个分支。

#### 参考：

https://stackoverflow.com/questions/8863917/importerror-no-module-named-pil

