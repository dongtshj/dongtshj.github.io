---
title: Lua遍历table的方式
tag: Lua
categories: Lua
date: 2019-03-17
---

### for语句 + #tab
---
这种方式只适合遍历被当作单纯的数组使用时的table，并且没有自定义元素的索引
```
# 先定义一个使用for语句 + #tab的遍历函数
function Traversal(tab) 
    print(#tab .. |)
    for i = 1, #tab do 
        print(tab[i]) 
    end 
end

local tab = {0, 1, 2, 5, 9, 7}
Traversal(tab)
# 打印结果：6|0 1 2 5 9 7
# 这个结果很符合预期

# 现在假设tab的内容是这样的：

tab = {0, 1, [4] = 2}
Traversal(tab)
# 打印结果：2|0 1

# 或者下面这样：

tab = {[2] = 0, 1, 2}
Traversal(tab)
# 打印结果：2|1 2

# 显然当tab的索引不连续时，这种方式只把连续的元素打印出来了，这是因为#tab的值只表示索引连续的元素个数，而不是tab的所有元素个数
```

### for语句 + pairs
---
这种方式几乎可以遍历各种形式的table，但是遍历的顺序可能不符合预期，因为它是按照key的hash值来顺序遍历的
```
function Traversal(tab) 
    for k, v in pairs(tab) do
        print(k .. ":" .. v)
    end
end

local tab = {0, 1, 2, 5, 9, 7} 
Traversal(tab)
# 打印结果：1:0 2:1 3:2 4:5 5:9 6:7

tab = {["ad"] = 0, 1, 2}
Traversal(tab)
# 打印结果：1:1 2:2 ad:0

tab = {"A", "B", [9] = "C", 1, [5] = "D", 12}
Traversal(tab)
# 打印结果：1:A 2:B 3:1 4:12 5:D 9:C

local tab = {[2]= "A", "B", [9] = "C", 1, [5] = "D", 12}
Traversal(tab)
# 打印结果：1:B 2:1 3:12 9:C 5:D
# 注意："A"没有打印出来，这就是为什么说它几乎能遍历所有形式的table,但是几乎没有会像上面那样使用table。。。
```

### for语句 + ipairs
---
这种方式只是形式上和for + pairs很像，内部机制差异很大。首先它很容易就会中断遍历操作（for + pairs几乎不会中断操作），但是它也有自己的优势：可以保证遍历的顺序。
```
function Traversal(tab) 
    for k, v in ipairs(tab) do
        print(k .. ":" .. v)
    end
end

local tab = {"A", 5, "D", 12}
Traversal(tab)
# 打印结果：1:A 2:5 3:D 4:12

tab = {0, 123, 12, "C", [7] = 3}
Traversal(tab)
# 打印结果：1:0 2:123 3:12 4:C
# 注意：在索引不连续时，中断了遍历

tab = {[2]= "A", "B", [9] = "C", 1, [5] = "D", 12}
Traversal(tab)
# 打印结果：1:B 2:1 3:12
```

### 总结
综上，在Lua的世界里，竟然找不到一个可以在任何情境下都能保证完全遍历出一个table！甚至在不要求遍历顺序的前提下，也找不到！为什么在一个编程语言里会出现这样的设计呢？要知道在像C++、JAVA之类的语言里，这简直是标配好不好！关你是普通数组还是关联数组，分分钟搞定。

在我看来，感觉是因为table在Lua里面实在是太灵活了！
* 没有类型限制，同一个table里可以同时存放字符、数值、函数甚至是另一个table！
* table里既可以放单个元素，也能存放key-value形式的键值对

就这么多了，记得选择合适自己需求的方式就行了