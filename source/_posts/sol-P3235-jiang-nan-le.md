---
author: a154051
avatar: 'https://a154051-img-1321592229.cos.ap-beijing.myqcloud.com/img/头像new.jpg'
title: P3235 [HNOI2014] 江南乐 
tags:
  - 博弈论
  - 整除分块
mathjax: true
date: 2023-12-06 17:30:38
categories: 题解
description: 博弈论加整除分块
keywords: 博弈论
photos: https://a154051-img-1321592229.cos.ap-beijing.myqcloud.com/img/23-12-6.jpg
---

[P3235 [HNOI2014] 江南乐](https://www.luogu.com.cn/problem/P3235)

蒟蒻居然不看题解写出来了黑题。

## 题意

首先给定一个数 $F$，有 $T$ 组数据，每组数据有 $n$ 堆石子，两个人进行博弈，每次从现有的石子堆中（包含分出来的石子堆）选择一堆石子数量不小于 $F$ 的石子堆，然后任选一个正整数 $M(M \ge 2)$，将其分为 $M$ 堆石子，并且这 $M$ 堆石子中石子数最多的一堆至多比石子数最少的一堆多 $1$（即分得尽量平均，事实上按照这样的分石子方法，选定 $M$ 和一堆石子后，它分出来的状态是固定的）。当一个玩家不能操作的时候，也就是当每一堆石子的数量都严格小于 $F$ 时，他就输掉。

所有数都是正整数，$1 \le T,N \le 100$，$1 \le F,\text{每堆石子数量} \le 10^5$。

## 题解

比较显然的是 $n$ 堆石子互不干扰，所以可以把每堆石子看作一个独立的游戏，最后把 $n$ 堆石子的 SG 值异或起来。

考虑求解一堆有 $i$ 个石子的 SG 值，记为 $SG_i$。显然 $\forall i<F,SG_i=0$。当 $i \ge F$ 时，朴素做法是枚举分出来的堆数 $k(2\le k \le i)$，对于每一个 $k$，令 $x \gets \lfloor \frac{i}{k} \rfloor,y \gets i \bmod k$，表示分出来的石子堆中，有 $y$ 堆包含 $x+1$ 个石子，$k-y$ 堆包含 $x$ 个石子，设这个局面的 SG 值为 $SG_
{i_k} =  \underbrace{SG_{x+1} \oplus \dots \oplus SG_{x+1}}_{\text{y个}} \oplus \underbrace{SG_{x} \oplus \dots \oplus SG_{x}}_{\text{k-y个}}$，则 $SG_i = \mathop{\operatorname{mex}}\limits_{j=2}^{i} SG_{i_j}$。

这样做的复杂度太高，注意到 $\lfloor \frac{i}{k} \rfloor$ 可以用整除分块，于是 $x$ 只有 $O(\sqrt{i})$ 种取值。对于每种取值，每多分出来一堆（如果可行的话），就相当于从 $y$ 中取 $x$ 个石子单独成一堆，该局面的 SG 值为 $y-x$ 个 $SG_{x+1}$ 与 $k-y+x+1$ 个 $SG_x$ 异或，也相当于将上一个局面的 SG 值与 $x$ 个 $SG_{x+1}$ 和 $x+1$ 个 $SG_x$ 异或，注意到后面两个的异或和只与 $x$ 的奇偶有关，如果 $x$ 是奇数则异或和是 $SG_{x+1}$，否则是 $SG_x$，由于 $x$ 不变，所以每个 $x$ 至多会产生两个 SG 值。

于是可以预处理出所有 SG 值，设最大的一堆有 $v$ 个石子，则时间复杂度为 $O(v\sqrt v)$

## 代码

```cpp
#include <bits/stdc++.h>
typedef long long ll;
using namespace std;
const int N=1e5;
int sg[N+7];
deque<int> q;
bool vis[N+7];
int main()
{
    int T,f;
    cin>>T>>f;
    for(int i=max(2,f);i<=N;++i) {  //注意这里至少要从2开始
        int l=2;
        vis[0]=1;q.push_back(0);
        while(1) {
            int x=i/l;
            if(!x)  break;
            int r=i/x,y=i-x*l;  //l是分出来的堆数，x是每堆至少多少石子，y是余下来的石子
            if(x<f-1)  break;
            int sum=0,tmp=0;
            if(y&1)  sum=sg[x+1];
            if((l-y)&1)  sum^=sg[x];
            if(!vis[sum])  vis[sum]=1,q.push_back(sum);
            if(x&1)  tmp=sg[x+1];
            else  tmp=sg[x];
            if(r-l+1>1) {
                sum^=tmp;
                if(!vis[tmp])  vis[sum]=1,q.push_back(sum);
            }  
            l=r+1;
        }
        for(int j=0;;++j)
            if(!vis[j]) {sg[i]=j;break;}
        for(int j=0;j<q.size();++j)  vis[q[j]]=0;
        q.clear();
    }
    while(T--) {
        int n;
        int ans=0;
        scanf("%d",&n);
        for(int i=1;i<=n;++i) {
            int x;
            scanf("%d",&x);
            ans^=sg[x];
        }
        ans=(ans!=0);
        printf("%d ",ans);
    }
    return 0;
}
```


