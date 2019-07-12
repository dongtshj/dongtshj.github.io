---
title: 安卓APP的安装目录
categories: Android
date: 2019-07-12
---

### 原由
---

在调试安卓APP涉及到缓存文件这块时，发现对APP文件的读写位置不太了解。就去网上查了查，然后总结（抄袭）一下吧（前人总结的确实已经够好了）。

### 涉及到的目录如下
---

* `system/app`：系统自带的应用程序，无法删除
* `data/app`：用户程序安装的目录，有删除权限。安装时把apk文件复制到此目录
* `data/data`：存放应用程序的数据
* `data/dalvik-cache`：将apk中的dex文件安装到dalvik-cache目录下(dex文件是dalvik虚拟机的可执行文件,其大小约为原始apk文件大小的四分之一)

### 安装与卸载过程
---

* **APP的安装** ：复制APK安装包到data/app目录下，解压并扫描安装包，把dex文件(Dalvik字节码)保存到dalvik-cache目录，并data/data目录下创建对应的应用数据目录
* **APP的卸载** ：删除安装过程中在上述三个目录下创建的文件及目录

**注意：没有Root的手机是看不到data目录下的内容的**

