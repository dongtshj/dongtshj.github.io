---
title: invalid arguments in function 'lua_cocos2dx_ui_Text_setString'
categories: cocos2dx-lua
date: 2019-05-23
---

项目中的代码类似这样：

`text:setString("xxx")`

类似的报错：

`invalid arguments in function 'bind_func_name'`

之前我一直以为是调用对象为 nil 的缘故，我就使劲的找啊找，我想这不可能啊，难道cocos2dx-lua自身又有啥bug？

我甚至在

`text:setString("xxx")`

之前调用了

`text:retain()`

以确保垃圾回收机制没有把它干掉了，我甚至在它的各级父节点都调用 retain() ，奈何始终还是有问题

尼玛后来发现是参数的问题。

`invalid arguments` 

我这四级英文真是白学了！
