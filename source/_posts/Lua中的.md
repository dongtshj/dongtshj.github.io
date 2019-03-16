---
title: Lua中加载代码的方式
tag: 
categories: Lua
date: 2019-03-16
---

### load (chunk [, chunkname [, mode [, env]]])
**加载一个代码块，参数chunk可以是字符串或者函数**
```
load("print(\"dongtshj.github.io\")")
> function: 0109b970 （一个函数地址）
load("print(\"dongtshj.github.io\")")()
> dongtshj.github.io （表达式加上了()表示调用了函数）
```
### loadfile ([filename [, mode [, env]]])
**和load类似，不过是从文件或标准输入中获取代码块**

假设test.lua文件内容如下：
```
print("dongtshj.github.io")
```
那么有：
```
loadfile("test.lua")
> function: 00634ef8 （一个函数地址）
loadfile("test.lua")()
> dongtshj.github.io
```
### dofile ([filename])
**和loadfile类似，只不过它打开并执行了文件中的Lua代码块**

还是拿上面那个test.lua文件举例
```
dofile("test.lua")
> dongtshj.github.io
```


