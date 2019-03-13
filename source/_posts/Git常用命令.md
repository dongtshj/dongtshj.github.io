---
title: Git常用命令
#tag: Git常用命令
categories: Git
date: 2019-03-13
---

### 生成ssh key
我是为了用来访问github上的repo，带.pub的是公钥，不带后缀的使密钥。把公钥内容粘贴到github的账户设置里就可以访问远程repo了,如果本地机器已经有了公钥，注意会提示你是否执行覆盖操作，然后一路回车就行了。
`ssh-keygen -t rsa -C "your_email@example.com"`

