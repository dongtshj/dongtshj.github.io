---
title: Lua的元表与元方法
tag: Lua
categories: Lua
date: 2019-03-18
---

### 元表（MetaTable）
---
* Lua中的每个值都可以有一个*元表*，这个*元表*就是一个普通的Lua表（table）；元表中的键所关联的那些值被称为*元方法*，而这些键则对应着不同的*事件名*
* 元表用于定义原始值在特定操作下的行为，有点像函数重写的效果一样。只是这里的原始函数就是个nil（根本就没有函数，或者说重写了一个内容为空的函数）
* 使用setmetatable来给Lua的一个表设置元表，用getmetatable来获取一个元表
* 表一般拥有独立的元表，而其它类型的值则是整个类型共享一个元表。注意：在标准Lua中，不可以改变除表以外的其它值的元表，比如number、string等

### 使用元表与元方法实现table的“操作符重载”
---
```
local tab1 = {1, 2, 3, 4}
local tab2 = {10, 20, 30, 40}

local metaTab = {
    __add = function (t1, t2)
        for i = 1, #t2 do
            table.insert(t1, t2[i])
        end
        return t1
    end
}

setmetatable(tab1, metaTab)
local tab3 = tab1 + tab2
-- tab3 = {1, 2, 3, 4, 10, 20, 30, 40}
```
这里的add表示“+”操作的事件名，__add是完成“+”操作的元方法的key值，这样我们就可以自定义两个表相加的操作了。类似的还有：
事件名|对应操作（元方法）的key值|含义
- | - | -
sub | __sub | 减法
mul | __mul | 乘法
div | __div | 除法
len | __len | 取长度
eq | __eq | 相等
index | __index | 索引table[key]
newindex | __newindex | 索引赋值 table[key] = value
call | __call | 函数调用操作

### __index与OOP中的继承
---
* 当你给一个表的元表设置了__index对应的域（元方法）时（注意，这里的元方法既可以是一个函数，也可以是另一个表），
* 当你对这个表索引一个key时，如果这个表里不存在这个key时，解释器就会去这个表的元表找__index对应的域（元方法）
* 如果元方法是一个函数，则以table和key作为参数调用它。如果是一个表，那就有趣了，就继续在__index对应的表里索引key
* 假如它的元表（元表就是普通的Lua表）又有自己的元表，如果当前还是没有索引到key，那就继续去元表的元表里去索引
* 直到索引到key，或者没有元表了，或者元表里没有再定义__index对应的域了

想想看，表与元表的关系，和子类与父类的关系是不是很像呢。当把一个表A设置成另一个表B的元表时，表B就相当于同时拥有了A和B的所有属性和操作！

然后还有那个__call，简直就是面向对象里给对象定义了调用运算符"()"
