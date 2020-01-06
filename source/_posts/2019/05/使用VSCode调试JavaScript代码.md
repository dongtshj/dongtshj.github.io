---
title: 使用VSCode调试JavaScript代码
categories: JavaScript
date: 2019-05-08
---

### 说明
---

* 这里有现成的模块：https://github.com/dongtshj/debugJS
* 想要执行、调试JavaScript代码，就需要JavaScript的解释器、调试器程序。这些都集成在了JavaScript引擎之中，而JavaScript引擎一般是集成在浏览器程序之中的，比如Google Chrome（V8引擎）、Mozilla Firefox（SpiderMonkey引擎）
* 这里是使用VSCode的一款微软开发的名为Debugger for Chrome的插件来完成使用chrome调试JavaScript代码

### 模板项目结构
---

```
|---debugJS
|   |---index.html
|   |---main.js
|   |---.vscode
|   |   |---launch.json
```

* 要想浏览器执行你的代码，首先需要一个HTML文件，项目里使用的是index.html
* 然后需要一个.js文件来存放JavaScript代码，项目里使用的是main.js
* 最后再把.js文件嵌入到HTML文件当中就可以顺利调试、执行了
* launch.json是用来配置启动参数的，内容如下：
```
{
    "version": "0.2.0",
    "configurations": [
        {
            "type": "chrome",
            "request": "launch",
            "name": "Launch Chrome",
            "sourceMaps": true,
            "file": "${workspaceFolder}/index.html"
        },
    ]
}
```

### Python和Lua
---
这两个就很简单了，没啥好说的，只要下载对应的VSCode插件就能运行、调试相应的代码了。


