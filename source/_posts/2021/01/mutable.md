---
title: mutable
categories: C++
date: 2021-01-30
---

我以为我一直都不会用到 C++ 中这个关键字。

最近还真的被我用到了。

我碰到了Xcode给我这样一个提示：
```
Cannot assign to a variable captured by copy in a non-mutable lambda
```

网上搜了下：因为默认情况下，Lambda 无法修改捕获列表中的变量。

关于Lambda的介绍：

---
* [ 捕获列表 ] ( 形参数列表 ) mutable(可选) 异常属性 -> 返回值类型 { 函数体 }	(1)

* [ capture-list ] ( params ) -> ret { body }	(2)

* [ capture-list ] ( params ) { body }	(3)

* [ capture-list ] { body }	(4)

(1)为完整的形式，包含变量捕获列表、形参列表、可变属性（可选）和返回值类型等。

(2)省略了mutable，表示Lambda不能修改捕获的变量。

(3)Lambda的返回值类型如果可以由函数体中的实际返回值推导出，可以省略。

(4)如果没有形参，可以省略圆括号。

---
