---
title: Git常用命令
tag: git
categories: Git
date: 2019-03-13
---

### 生成ssh key
---
我是为了用来访问github上的repo，带.pub的是公钥，不带后缀的使密钥。把公钥内容粘贴到github的账户设置里就可以访问远程repo了,如果本地机器已经有了公钥，注意会提示你是否执行覆盖操作，然后一路回车就行了。
`ssh-keygen -t rsa -C "your_email@example.com"`

### 新建分支
---
* 新建分支但不切换（依然停留在当前分支）
`git branch new_branch_name`
* 切换分支
`git checkout new_branch_name`
* 新建分支并切换
`git checkout -b new_branch_name`