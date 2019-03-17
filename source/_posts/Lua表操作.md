---
title: Lua表操作
tag: Lua
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

* **table.insert(tab, [pos,] value)**
在tab的pos处插入value，并后移tab[pos],tab[pos + 1],...,tab[#tab]。默认插入到tab末尾,即pos的默认值为#tab + 1
```
local tab = {1, 2, 3}
table,insert(tab, 7)
print(tab[4])
> 7
```

* **table.remove(tab [,pos])**
移除tab中pos位置上的元素，并返回移除的值。默认移除最后一个元素
```
local tab = {1, 2, 3, 7}
table.remove(tab)
# 现在tab的内容：{1, 2, 3} 
```

* **table.sort(tab [, comp])**
对tab进行排序，可自定义排序规则
```
local tab = {5, 2, 1, 7, 13}
local comp = function(l, r) return l > r end
table.sort(tab, comp)
# 现在tab的内容：{13, 7, 5, 2, 1}
```

* **table.concat(tab [, sep [, i [, j]]])**
连接tab的各个元素，以sep分割各个元素,返回值为：
`tab[i] .. sep .. tab[i + 1] ··· sep .. tab[j]`
其中的各项参数默认值为：sep = "", i = 1, j = #tab
```
local tab = {"A", "B", "C"}
print(table.concat(tab, "|"))
> A|B|C
print(table.concat(tab, ""))
> ABC
```

* **table.move(tab1, begin1, end1, begin2 [, tab2])**
将tab1中的元素移到tab2中，等同于下面这个多重赋值操作：
`tab2[begin2], ··· = tab1[begin1], ··· , tab1[end1]`
其中tab2的默认值为tab1。*其实感觉move这个函数名不够好，move给人的感觉是，拷贝后会把自己的给删除了；或者就是把自己的元素给别人了，自己不再拥有这些元素的控制权了。其实这里就是个单纯的拷贝操作*
```
local tab1 = {1, 2}
local tab2 = {}
table.move(tab1, 1, 2, 1, tab2)
# 现在tab1的内容：{1, 2}
# 现在tab2的内容：{1, 2}
```

* **table.pack(...)**
构建新的table
```
local tab = table.pack("A", "B", "C")
# 现在表tab的内容：{"A", "B", "C"}
```

* **table.unpack(tab [, i [, j]])**
返回table中的元素，i默认为1，j默认为#tab；等价于
`return tab[i], tab[i + 1], ··· ,tab[j]`
