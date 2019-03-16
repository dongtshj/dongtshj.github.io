---
title: Lua中加载代码的方式
#tag: 
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

**loadfile从硬盘上加载lua文件，并把它编译成lua代码块**

假设test.lua文件内容如下：
`print("dongtshj.github.io")`
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
### require
* require是用来加载**Lua模块**的，而不单单是.lua文件。Lua模块可以是一般的.Lua文件，但也可以是.dll或者.so文件
* require会把加载起来的模块缓存起来，所以再次加载同名模块时，只是把首次加载的缓存返回了。如果你确实想要重新加载的话，请先清除缓存
* require具体的功能取决于package.loaders（Lua 5.1）或者package.searchers（Lua 5.2/5.3）

### import
Lua标准库里并没有这个函数，一般是项目自身框架提供的功能性函数，所以它的功能取决于具体的实现

