---
title: Linux常用命令
tag: Shell
categories: Linux
date: 2019-02-21
---

### 从一台Linux远程机器登录到另一台Linux远程机器
---
* 完整的命令格式
`ssh [-l login_name] [-p port] [user@]hostname `
* 最常用的形式
`ssh root@192.168.0.1`
* 退出登录
`exit`

### 从一台Linux远程机器拷贝文件到另一台Linux远程机器
---
这里使用scp命令，它是secure copy的缩写，中文名：安全拷贝，关于这个命令的更详细的介绍[**点这里**](https://linuxtools-rst.readthedocs.io/zh_CN/latest/tool/scp.html)
#### 复制到远程
* 复制文件
`scp local_file remote_username@remote_ip:remote_file`
* 复制文件到目录
`scp local_file remote_username@remote_ip:remote_folder`
* 复制目录
`scp -r local_folder remote_username@remote_ip:remote_folder`

#### 从远程复制
从远程复制到本地的scp命令与上面的命令一样，只要将从本地复制到远程的命令后面2个参数互换顺序就行了。
* 复制文件
`scp root@10.6.159.147:/opt/soft/demo.tar /opt/soft/`
* 复制目录
`scp -r root@10.6.159.147:/opt/soft/test /opt/soft/`

### 移动/重命名文件
---
`mv [options] SourceFile/SourceDirectory TargetFile/TargetDirectory`
* 重命名文件
`mv test.log test1.txt`
* 移动当前目录下的所有文件或目录到上层目录
`mv * ../`

### 删除文件/文件夹命令
---
`rm [options] file/directory`
* 删除文件
`rm testFile.txt`
* 删除目录：-r表示递归删除，删除目录时必须要加上这个选项
`rm -r testDirectory`