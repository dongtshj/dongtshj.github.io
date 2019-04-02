---
title: 使用logcat查看android系统日志
tag: Debug
categories: Android1
date: 2019-03-13
---

对我这个做游戏开发的来说，在android平台上通常会遇到的棘手情况如下
* 安卓游戏程序通常通过JNI技术调用游戏的C++代码，所以在eclipse或者android studio上根本没法断点调试
* 可能会遇到开发环境下（win32）运行正常，但是打包后在android上跑时就会出现问题
* SDK相关模块的调试只能在真机上进行调试

### android系统日志
---
这个时候只能靠低效的查看日志来定位问题了，还好android的系统在遇到错误时，打印出来的堆栈信息相当完整，也能打印出编程语言提供的log类函数的输出。

* 这个日志有个好处就是即使当时没有输出系统日志，你也可以在随后的时间里打印出以前的日志来分析问题
* 但是，请注意，android的系统日志是存在一个环形缓冲区上的，所以当测试在真机上重现出一个不常见的bug时，最好不要让测试继续再操作这台设备，以防导致问题的日志被后续日志覆盖。

### 查看android系统日志
---
我之前都是开着eclipse或者android studio，然后手机使用数据线连着计算机。这样就可以在IED的logcat窗口里查看手机运行时的系统日志了。后来发现使用adb可以在WLAN环境下远程获取手机的系统日志！也可以选择实时打印。

### 常用的操作
---
* 查看adb当前连接的设备
`adb devices`
* 使用logcat输出日志
`adb logcat`
* 输出的日志将会带上打印日志时的时间
`adb logcat -v time`
* 将日志重定向到一个文件中
`adb logcat -v time > log.txt`

### release_print而不是print
---
我发现cocos2dx-lua里的print打印的日志，使用logcat竟然输出不了，换成release_print才行。

### 使用adb把命令发往指定的设备
---
开发的pc通过USB连接多台安卓设备或者pc上运行多个安卓模拟器时，执行
`adb devices`
命令查看，发现list of devices attached列表下有多台设备地址。这个时候直接执行像`adb logcat`是不行的，因为adb不知道你是要往哪台设备发送命令。所以需要加上参数指定哪台设备
`adb -s 127.0.0.1:21503 logcat`
才行
