---
title: SVN主干（trunk）与分支（branch）的合并操作（svn merge）
tag: SVN
categories: 工作
date: 2019-03-01
---

* 网上搜索了很多关于这个主题的文章，个人觉得最好的就是[这个了](https://www.centos.bz/2017/08/svn-merge-operate/)

* 关于更详细的说明看这里的[中文文档](http://svnbook.red-bean.com/nightly/zh/svn.branchmerge.html)

## 把分支（branch）合并到主干（trunk）
* 首先到你的trunk根目录下（本地的working copy），更新服务器上最新代码，提交本地还未提交的代码，解决存在的冲突。
* 鼠标空白处右键，选择"TortoiseSVN"选项，然后选择“Merge...”选项
* 在弹出的“Merge type”界面里：选择“Merger a range of revisions”选项
* 在弹出的“Merge revision range”界面里：
    * “URL to merge from”处，选择你想要合并到主干（trunk）的那个分支（branch）的路径
    * “Revision range to merge”处，选择你想要合并的版本号 （这些都是对分支（trunk）的修改产生的版本号）
    * 点击“下一步（Next）”按钮
* 在“Merge options”界面里：
    * 使用默认的合并选项就可以了
    * 合并之前可以先点击“测试合并（Test merge）”按钮，测试一下合并结果如何
    * 确保没有问题后，点击“合并（Merge）”按钮进行合并
    * 如何产生冲突了也不用担心，可以选择手动解决冲突或者撤销此次合并操作
    * 合并后记得提交合并后的结果

## 把主干（trunk）合并到分支（branch）
* 操作简直一模一样的：只是把选项“URL to merge from”处，改成选择你要合并到分支（branch）的那个主干（trunk）的路径

## 注意整个合并过程其实可以分解成：**diff + apply**这两个操作的组合
* **diff**：既可以是指比较同一个tree的不同版本之间的差异（“Merger a range of revisions”），也可以是比较两个tree之间的差异（“2-URL Merges”）
* **apply**：就是把这些差异应用到那你想要的working copy上