---
title: IOS打包生成的dSYM文件
categories: IOS
date: 2019-07-24
---

### dSYM文件
---

需要这个文件是因为：bugly需要每次IOS打包生成的dSYM文件，以提取对应的符号表。当bugly记录到APP崩溃时，根据这个符号表可以还原成人类可读的崩溃堆栈信息。没有这个符号表时，bugly后台看到的只是一些十六进制的内存地址；而有了这个符号表就可以还原成崩溃发生在哪个代码文件的哪一行。这样定位问题就非常方便了（某些崩溃即使在有符号表的情况下，依然是一堆十六进制符号。。。）

### 定位dSYM文件
---

#### 那么问题来了，IOS打包后这个文件生成的位置在哪呢？
*因为参考了bugly官方文档，我被这个问题耗了不少时间！*根据bugly官方文档说明，这个文件可以这样找到：
* 打开项目的XCode工程
* 在`Products`文件夹下，找到`Test.app`
* 右键，选择`Show in Finder`，这样就能找到dSYM文件了

问题是，我发现这个文件在每次打包（`Window` -> `Archive`）时，并不会自动生成。于是Google一下，发现release模式下是会默认生成这个文件，而debug模式下则不会。

我想啊，难道我们项目打包都是用的debug？

那不太可能吧，不过实在没办法。照网上说法：选中XCode工程的 `target` -> `Build Setting` -> `Debug Information Fromat`，在 `Release` 项选择 `DWARF with dSYM File` 。

* 问题是，我这样设置了，Archive后还是没有生成新的dSYM文件
* 旁边同事说试试删掉旧的dSYM文件呢？我删了后，依旧不会生成新的dSYM文件
* 最后我发现，执行XCode工程的 `Products` -> `Clean` 命令，然后再执行 `Products` -> `Build` 命令。构建结束后，确实生成了新的dSYM文件

但是呢，问题又来了。提取这个dSYM文件里的符号表上传到bugly后，发现和APP的崩溃堆栈根本对不上，也就是不匹配的意思；这说明这里生成的dSYM文件并不是我需要的那个dSYM文件！

#### 真正需要的dSYM文件位置
在XCode工程中，选择 `Window` -> `Organizer` -> `Archives` ，右键选择 `Show in Finder` ，再显示包内容，打开dSYMs文件夹，里面就有打包（`Window` -> `Archive`）对应生成的 `Test.app.dSYM` 文件了: )