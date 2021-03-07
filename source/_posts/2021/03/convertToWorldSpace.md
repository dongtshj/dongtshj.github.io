---
title: convertToWorldSpace
categories: cocos2dx
date: 2021-03-07
---

### 与convertToWorldSpaceAR的区别
---
- 不带AR的版本，会忽略掉Node的锚点设置，以左下角作为转换时的位置。
- 带AR的版本，会以Node的锚点作为转换时的位置。

### 目标的缩放会影响转换结果
---
在对Node进行与convertToWorldSpace调用时，如果Node转换前后的缩放发生了变化，那么会导致转换后的位置出现偏差。

### contentsize不需要再乘以缩放
---
使用不带AR的版本，以Node的中间作为转换时的位置时，可能会写成

```node->convertToWorldSpace(node->getContentSize() * node->getScale() / 2);
```

这里实际上不需要考虑到node的缩放影响。可能后面计算的时候自身transform已经包含这个缩放信息了。

### 锚点是否影响节点位置以及透明度是否级联，都是可以通过参数控制的
---
这个之前还真没用过。

- setCascadeColorEnabled
- setCascadeOpacityEnabled	

### Layer的位置不受锚点影响
---
这个之前也没有注意到，还有就是Node的默认锚点是0，0。而Sprite的默认锚点是：0.5，0.5


