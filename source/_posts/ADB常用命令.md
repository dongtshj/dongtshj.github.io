---
title: ADB常用命令
tag: Debug
categories: Android
date: 2019-03-13
---

### pull / push
---

* 从模拟器或设备复制文件
`adb pull remote local`
* 将文件复制到模拟器或设备
`adb push local remote`

local 和 remote 指的是开发计算机（本地）和模拟器/设备实例（远程）上目标文件/目录的路径。

### 发送 shell 命令
---

* 可以选择进入或者不进入模拟器/设备实例上的 adb 远程 shell
* 要在不进入远程 shell 的情况下发出一个 shell 命令，在命令的前面加上 `adb shell` 即可，像这样： `adb shell shell_command` ，或者直接使用命令 `adb shell` 进入远程 shell 后，再进行其它操作。

### 其它命令
* `screencap` 截屏： `adb shell screencap /sdcard/screen.png`
* `screenrecord` 录屏： `adb shell screenrecord /sdcard/demo.mp4`