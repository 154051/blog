---
author: 154051
avatar: https://a154051-img-1321592229.cos.ap-beijing.myqcloud.com/img/头像new.jpg
title: 5.2模拟赛总结
tags:
  - 模拟赛
mathjax: true
date: 2021-05-02 18:33:00
categories: 总结
description: 5.2模拟赛总结
keywords: 模拟赛总结
photos: https://a154051-img-1321592229.cos.ap-beijing.myqcloud.com/img/5-2.jpg
---

## [T1](https://www.luogu.com.cn/problem/P6225)

看完题手算了几组样例，找了找规律，先写了前4档，想到了树状数组，但没写，先写后面了。

## [T2](https://www.luogu.com.cn/problem/P6273)

先打了暴力，想了想似乎可以存每个字符出现的次数然后转移到下个状态，但是打起来比较麻烦，就暂时放弃。

## [T3](https://www.luogu.com.cn/problem/P6116)

推了一遍样例，想到一个贪心，打完过了样例，就不管了。

## [T4](https://www.luogu.com.cn/problem/P6274)

第一眼看上去不太可做，再想想发现能推出来第一档分的公式，但时间不够了，最后没写出来。

## 赛后

T1判断区间长度是奇数还是偶数的时候我写了 `if((r-l+1)&1==0)`，但是 `&` 的优先级比 `==` 低，所以用这个式子判断的长度永远是奇数，但是样例里区间长度都是奇数，而且我也没有造其他数据，于是就拿到了0分的好成绩。

T2暴力打错了，T3贪心写假了。

**全 部 木 大**

这个故事告诉我们**考试时一定要对拍**，就算不对拍也要造几组数据，不然会有爆蛋的风险。