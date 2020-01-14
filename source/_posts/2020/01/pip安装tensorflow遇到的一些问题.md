---
title: pip安装tensorflow遇到的一些问题
categories: Python
date: 2020-01-14
---

### Could not find a version that satisfies the requirement tensorflow
---

使用`python`的`pip`安装`tensorflow`提示：

`Could not find a version that satisfies the requirement tensorflow`

后来发现，`tensorflow`只支持64位的`python`。

换成最新的64位`python`后，发现还提示这个错误。

又发现`tensorflow`只支持64位的`python 3.6`。

最新的64位`python 3.8`还不支持。。。

### Could not find a version that satisfies the requirement cv2
---

直接执行安装命令：

`pip install cv2`

提示：

`Could not find a version that satisfies the requirement cv2`

原来`cv2`根本不是一个`python`软件的包名，`cv2`只是`opencv-python`这个`python`软件包的一个组成部分。

换成：

`pip install opencv-python`

就好了

### 参考：
* https://stackoverflow.com/questions/48720833/could-not-find-a-version-that-satisfies-the-requirement-tensorflow
* https://stackoverflow.com/questions/37776228/pycharm-python-opencv-and-cv2-install-error




