---
author: 154051
avatar: 'https://a154051-img-1321592229.cos.ap-beijing.myqcloud.com/img/头像new.jpg'
title: 4.29模拟赛总结
tags:
  - 模拟赛
mathjax: true
date: 2021-04-29 20:25:48
categories: 总结
description: 4.29模拟赛总结
keywords: 模拟赛总结
photos: https://a154051-img-1321592229.cos.ap-beijing.myqcloud.com/img/4-29new.jpg
---

## T1

读完题后发现整张图是个 DAG，可以拓扑排序+DP，应该能过6个点。

## T2

一开始想到了容斥，推了半天没推出来，就放弃了，打了个 $O(m^n)$ 的暴力，能拿 30分。

## T3

先打了个暴力，然而只有10分。

有20分是一条链，想到了分块的做法。修改操作整块打标记，散块暴力修改，查询就整块跳，如果整个块都是可行就累加上，有不可行的就停止直接暴力扫。虽然想出来了但是细节巨多，最后没有调出来。

## 总结

最后20分没拿到有点亏。算法能力有待提升，比如线段树二分之前就不知道。