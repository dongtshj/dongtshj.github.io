---
title: Lua字符串操作
tag: string
categories: Lua
date: 2019-03-09
---

### 替换操作
`string.gsub(s, pattern, repl [, n])`
* s：想要被操作的整个字符串
* pattern：整个字符串中想要被替换掉的字符串
* repl：替换上面pattern的字符串
* [, n]：中括号表示可选的参数n，替换n次；忽略则全部替换
* 返回值：替换后的字符串和替换的次数

```
> string.gsub("dongtshj.github.io", ".", '')
dongtshjgithubio 2

> string.gsub("dongtshj.github.io", ".", "", 1)
dongtshjgithub.io 1

> string.gsub("dongtshj.github.io", "k", "")
dongtshj.github.io 0
```

### 查找操作
`string.find(s, pattern [, init [, plain]])`
* s：想要被操作的整个字符串
* pattern：要查找的字符串
* [, init [, plain]]：可选参数，init表示搜索起始位置，默认为1
* 返回值：如果找到一个匹配，则返回它在s中的起始位置；否则，返回nil

```
> string.find("dongtshj.github.io", "git")
10 12
> string.find("dongtshj.github.io", "kkk")
nil
```

### 字符串格式化
`string.format(formatString, ...)`
* 返回值：格式化后的字符串formatString

```
local str = string.format("It is now %d.", 2019)
print(str)
> It is now 2019.
```

### 字符串截取
`string.sub(s, i [, j])`
* 返回s的子串，该子串在s中的位置为[i, j]，j的默认值为-1（s的末尾）
* string.sub(s, 1, j) 返回s的[1, j]部分：长度为j的前缀串
* string.sub(s, -i) 返回s的[i, end]：长度为i的后缀串
```
string.sub("dongtshj.github.io", 1, 8)
> dongtshj
string.sub("dongtshj.github.io", -2)
> io
```

### 反转字符串
`string.reverse(s)`
* 返回s字符序列的逆序列
```
string.reverse("dongtshj.github.io")
> oi.buhtig.jhstgnod
```

### 字符串长度
`string.len(s)`
* 返回字符串s的长度
```
string.len("dongtshj.github.io")
> 18
```

### 字符串转为大写
`string.upper(s)`
* 将s中的小写字符都转为大写，然后返回转换后的字符串
```
string.upper("dongtshj.github.io")
> DONGTSHJ.GITHUB.IO
```

### 字符串转为小写
`string.lower(s)`
* 将s中的大写字符都转为小写，然后返回转换后的字符串
```
string.lower("DONGTSHJ.GITHUB.IO")
> dongtshj.github.io
```

### 将数字转换成字符
`string.char(num)`
```
string.char(97)
> a
```

### 将字符转换成数字
`string.byte(char)`
```
string.byte(a)
> 97
```

### 字符串n次拷贝
`string.rep(s, n [, sep])`
* 返回以sep为连接符的s的n次拷贝，sep默认为空，即没有任何连接符
```
string.rep("dongtshj", 3, "|")
> dongtshj|dongtshj|dongtshj
```

## 待填的坑
`string.dump(function [, strip])`

`string.gmatch (s, pattern)`

`string.match (s, pattern [, init])`

`string.pack (fmt, v1, v2, ···)`

`string.packsize (fmt)`

`string.unpack (fmt, s [, pos])`
