---
title: Lua字符串操作
tag: string
categories: Lua
date: 2019-02-25
---

### 替换操作
`string.gsub (s, pattern, repl [, n])`
* s：想要被操作的整个字符串
* pattern：整个字符串中想要被替换掉的字符串
* repl：替换上面pattern的字符串
* [, n]：中括号表示可选的参数n，替换n次；忽略则全部替换
* 返回值：替换后的字符串和替换的次数

`> string.gsub("dongtshj.github.io", ".", '')`

`dongtshjgithubio 2`

`> string.gsub("dongtshj.github.io", ".", "", 1)`

`dongtshjgithub.io 1`

`> string.gsub("dongtshj.github.io", "k", "")`

`dongtshj.github.io 0`

### 查找操作
`string.find (s, pattern [, init [, plain]])`
* s：想要被操作的整个字符串
* pattern：要查找的字符串
* [, init [, plain]]：可选参数，init表示搜索起始位置，默认为1
* 返回值：如果找到一个匹配，则返回它在s中的起始位置；否则，返回nil

`> string.find("dongtshj.github.io", "git")`

`10 12`

`> string.find("dongtshj.github.io", "kkk")`

`nil`