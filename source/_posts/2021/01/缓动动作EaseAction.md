---
title: 缓动动作EaseAction
categories: cocos2dx
date: 2021-01-31
---

* 直接使用cocos2dx动作系统的 MoveTo、ScaleTo、SkewTo、FadeTo、RotateTo虽然也能完成基本的功能需求，但是这样匀速的变化过程，略显生硬。

* 要想让玩家看到*舒服*的变化过程，还需要对基本的动作添加一些额外的修饰。
这里就要用到被 **缓动函数** 包裹修饰后的 **缓动动作**

* 你可以在[这个网页的最下面](https://docs.cocos.com/creator/manual/zh/scripting/actions.html)找到cocos2dx支持的所有缓动动作的列表。
