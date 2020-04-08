---
title: STL中的map与hash_map
categories: C++
date: 2020-04-08
---

### map
STL的map底层是用红黑树存储的，查找时间复杂度是log(n)级别

### hash_map
STL的hash_map底层是用hash表存储的，查询时间复杂度是常数级别

### 什么时候用map，什么时候用hash_map?

这个要看具体的应用，不一定常数级别的hash_map一定比log(n)级别的map要好，hash_map的hash函数以及解决地址冲突等都要耗时，而且众所周知hash表是以空间效率来换时间效率的，因而hash_map的内存消耗肯定要大。一般情况下，如果记录数非常大时，考虑hash_map，查找效率会高很多，如果要考虑内存消耗，则要谨慎使用hash_map。