---
title: 分析Lua在bugly上的报错
categories: Lua
date: 2020-01-16
---

### module 'xxx' not found: no field package.preload[xxx]
---

完整一点的报错信息：
```
module 'xxx' not found: 
no field package.preload['xxx'] 
no file './xxx.lua' 
no file '/usr/local/share/luajit-2.1.0-beta2/xxx.lua' 
no file '/usr/local/share/lua/5.1/xxx.lua' 
no file '/usr/local/share/lua/5.1/xxx/init.lua' 
no file '/data/user/0/xxx.lua' 
no file '/data/user/0/xxx.lua' 
no file '/data/user/0/xxx.lua' 
no file './xxx.so' 
no file '/usr/local/lib/lua/5.1/xxx.so' 
no file '/usr/local...too long be cutted!
```

从字面意思来看，就是找不到名为xxx的lua模块。

问题来了，为什么安装包里的某个文件会不翼而飞呢？

是用户删掉了？还是包体不完整？

我这里遇到的情况是这样的：

这个名为xxx的lua文件，需要通过热更下载到用户的设备上。而热更新系统有一套方法来判断：
* 哪些文件本地没有，因此需要从服务器上下载这个文件到用户设备上
* 哪些文件本地和服务器上有差异，这个时候也需要重新下载这个文件替换本地的旧版本

一般来说，热更系统需要维护一个列表文件，通过这个文件它就可以回答上面的两个问题。

问题就出在这个列表文件上，列表文件是通过记录所有需要热更文件的md5码，来判定是否需要更新某个文件的。

而我的安装包里的列表文件记录了xxx.lua已经是最新了的，它的md5码和服务器上的一模一样。但是实际上本地根本没有这个叫做xxx.lua的文件。

根本上说，就是信息的元数据（热更系统维护的那个列表文件）和信息实体（xxx.lua文件），出现了差错，已经不一致了。

数据的元数据已经不能准确的反映它所指代的数据实体了。

才导致这样的错误。

### 定义的lua变量，无缘无故的就成nil值了，消失了。彷佛从来就没有过这个变量一样
---

出现这种情况的原因之一可能是：

热更或者其它相关系统或者开发工具，根本没有检测到你添加代码所作的改动，所以根本就没通过热更把你的最新代码下载到用户的设备上。

这个情况和我之前在C++开发环境下遇到过的有点类似。

我在某个cpp文件添加了一些代码，编译后，从运行效果来看，就好像根本没有我新添加的代码一样。

而且，编译时，发现visual studio输出窗口显示：没有检测到任何改动。。。

一般这种情况下，打开你添加代码的那个文件，按回车键，添加几个空行保存后，重新编译运行或者打包热更就好了。




