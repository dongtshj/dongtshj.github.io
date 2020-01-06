---
title: Clamp函数
categories: C++
date: 2020-01-06
---

#### Clamp函数可以将随机变化的数值限制在一个给定的区间[min, max]内：

```
template<class T>
T Clamp(T x, T min, T max)
{
    if (x > max)
        return max;
    if (x < min)
        return min;
    return x;
}
```

#### 刚开始工作的时候，光看这个函数的名字，我在一段时间里，有好几次用翻译软件看了下翻译，楞是很长时间不知道这个函数到底是用来干嘛的。。。

#### 谷歌翻译给出的翻译如下：
![clamp.PNG](https://i.loli.net/2020/01/06/k1IR5a3wYWxXVZL.png)