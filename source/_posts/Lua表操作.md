---
title: Lua表操作
tag: lua
categories: Lua
date: 2019-03-17
---

* Lua中的表即table，它是Lua提供的唯一一个复杂数据结构。
* Lua中的table可以用来表示数组或者关联数组等常见概念。
* Lua中的模块、包以及面向对象等概念也是靠table来实现的。

### 初始化
---
可以这样：
```
local table = {}
table[1] = "A"
table[2] = "B"
table[3] = "C"
```
也可以这样：
```
local table = {
    [1] = "A",
    [2] = "B",
    [3] = "C"
}
```
或者这样：
```
local table = {
    "A",
    "B",
    "C"
}
```

### Table操作
---
* table.insert(list, [pos,] value)
在list的pos处插入value，并后移list[pos],list[pos + 1],...,list[#list]。默认插入到list末尾
```
local tab = {1, 2, 3}
table,insert(tab, 7)
print(tab[4])
> 7
```
* table.remove(list [,pos])
移除list中pos位置上的元素，并返回移除的值。默认移除最后一个元素
```
local tab = {1, 2, 3, 7}
table.remove(tab)
# 现在tab的内容：
tab = {1, 2, 3} 
```