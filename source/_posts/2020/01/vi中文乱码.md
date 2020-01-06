---
title: vi中文乱码
categories: Linux
date: 2020-01-06
---


#### 问题：vi/vim 编辑ANSI文本时，中文会显示乱码！

#### 解决方法：修改vi/vim配置文件

* vi  配置文件路径：`/etc/virc`
* vim 配置文件路径：`/etc/vimrc`

#### 更改之前：

```

if v:lang =~ "utf8$" || v:lang =~ "UTF-8$"
set fileencodings=ucs-bom,utf-8,latin1,gbk
endif

```

#### 更改之后：

```
if v:lang =~ "utf8$" || v:lang =~ "UTF-8$"
set fileencodings=ucs-bom,utf-8,latin1,gbk,cp936,gb2312
set termencoding=utf-8
set fileformats=unix
set encoding=prc
endif
```

#### 之前百度一大圈，都是说加上：

```
set fileencodings=ucs-bom,utf-8,latin1,gbk,cp936,gb2312
set termencoding=utf-8
set fileformats=unix
set encoding=prc
```

#### 这几行。

#### 尼玛，就是没说必须要加在`if`和`endif`之间。。。