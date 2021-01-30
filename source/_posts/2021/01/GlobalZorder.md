---
title: GlobalZorder
categories: cocos2dx
date: 2021-01-30
---

# GlobalZorder

如果碰到这样的奇怪需求，比如想让父节点显示在子节点的上面，或者其它不按常理的层级关系显示的时候。你就可以通过 setGlobalZorder 设置一个节点的 GlobalZorder 属性。

所有节点默认的 GlobalZorder 的值为0，所以你只要将一个节点的 GlobalZorder 值设置为 1，就可以将它显示在所有的节点上面，而不用理会这些节点的层级关系。

设置为-1，也可以将此节点设置在所有其它节点的下面。
