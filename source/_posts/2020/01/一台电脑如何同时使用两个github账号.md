---
title: 一台电脑如何同时使用两个github账号
categories: 版本控制
date: 2020-01-07
---

其实很简单。

不要使用基于`SSH`协议的方式和`github`进行交互。

而是换成`HTTPS`的方式。

这样的话，就可以在每个项目的`.git/config`里，单独配置访问`github`的相关账号配置，缺点是可能会暴露你的`github`的明文密码。但是如果你不在`.git/config`里配置`github`账号的密码的话，也可以在每次提交的时候输入密码，只不过显示有点麻烦。

这个时候项目的`url`应该配置成：

`https://YourGithubName:YourGithubPassword@github.com/YourGithubName/YourProjectName.git`

不想把你的`github`密码暴露出来的话把上面的`:YourGithubPassword`去掉就可以了，这个时候每次提交需要你手动输入密码。

如何你采用了上面的建议的话，记得不要执行下面的`git`命令：

```
git config --global user.name "John Doe"
git config --global user.email "johndoe@example.com"
```

如果你已经执行过了，那可以执行：

```
git config --global --unset user.name "John Doe"
git config --global --unset user.email "johndoe@example.com"
```

来撤销上面的全局设置。

`SSH`的方式也许可以单独配置每个项目，但是我尝试过好几次，都失败了。

各种权限错误、拒绝访问。

感觉像是账号和项目都串门了。



