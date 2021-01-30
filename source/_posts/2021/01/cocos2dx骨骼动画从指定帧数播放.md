---
title: cocos2dx骨骼动画从指定帧数播放
categories: cocos2dx
date: 2021-01-30
---

# SkeletonAnimation

使用这个类来实现骨骼动画的播放时，有时我们需要动画不是从第1帧开始播放，而是在指定帧数播放。这个时候就需要用到下面这个接口：

```
virtual void update (float deltaTime);
```

我在工作中用到的场景，仅仅是想把两个相同的骨骼动画错开播放，所以随便指定一个不同的 deltaTime 就好了。

如果你想要在指定帧数播放的话，可以把帧数转换成时间，传入 update 即可。

上面是我暂时能想到的方法，也没有找出更好的解决方案了。

