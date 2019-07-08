---
title: “此应用专为旧版Android打造，因此可能无法正常运行。请尝试检查更新或与开发者联系”
categories: Android
date: 2019-07-08
---

### 说明
正在开发的游戏在 android 9 上出现过这个提示，但是只要用户忽略（点击提示框上的 “确定” 按钮）这个弹框就能正常进入游戏，所以也就没管它了。后来出现一例直接闪退的。。。

### 解决方法

* 如果你是使用 eclipse 打包 apk 的话（比如我现在就是用  eclipse 的，虽然自己也摸索过 Android Studio ）。请编辑[应用清单文件](https://developer.android.com/guide/topics/manifest/manifest-intro.html?hl=zh-cn) AndroidManifest.xml 里的 uses-sdk 字段，增加 `android:targetSdkVersion="22" ` 这样一对键值对（按照网上的说法键值对的值只要大于等于17就行，我参考公司其他项目设置成了22）进去，然后重新打包就行了。

* 如果你使用 Android Studio 打包 apk 的话，参考[这篇文章](https://blog.csdn.net/qiaoquan3/article/details/70185693)
