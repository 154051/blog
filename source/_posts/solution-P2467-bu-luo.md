---
author: a154051
avatar: https://a154051-img-1321592229.cos.ap-beijing.myqcloud.com/img/头像new.jpg
title: 【题解】P2467 [SDOI2010]地精部落
mathjax: true
date: 2021-03-03 21:40:32
tags: 
 - DP
categories: 题解
description: 前缀和优化DP
photos: https://a154051-img-1321592229.cos.ap-beijing.myqcloud.com/img/03-03.jpg
---

[P2467 [SDOI2010]地精部落](https://www.luogu.com.cn/problem/P2467)

# 题意

给 $n$ 座高度连续的山脉，即高度为 $1,2,3,···,n$，求有几种方案使得山脉的高度一高一低或一低一高（即波动数列）。答案对 $p$ 取模。

# 分析

显然，若要求 $k$ 座山脉的波动数列，无论它们的高度是否连续，只要高度互不相同，方案数都是一样的。

举个例子：求 $3$ 座山脉的波动数列，$(1,2,3)$ 和 $(1,3,5)$ 和 $(15,40,51)$的方案数都是一样的。

定义 $f(i,j,0)$ 表示前 $i$ 座山脉中，排名为 $j$ 的山脉放在最后面并且是山谷的方案数（$1$ 表示是山峰）。则最后答案为 $\sum_{j=1}^{n} f(n,j,0)+f(n,j,1)$。

若 $j$ 是山谷，则方案为剩下的 $i-1$ 座山脉中排名从 $j$ 到 $i-1$ 的山脉放后面并且做山峰的方案之和。$j$ 是山峰也同理。

所以有状态转移方程：

$$f(i,j,1)=\sum\limits_{k=1}^{j-1}f(i-1,k,0)$$

$$f(i,j,0)=\sum\limits_{k=j}^{i-1}f(i-1,k,1)$$

时间复杂度为 $O(n^3)$，会超时。

可以发现最里面一层循环可以用前缀和优化。

定义 $sum0(i,j)=\sum\limits_{x=1}^{j}f(i,x,0)$，$sum1(i,j)$ 同理，则原转移方程可化为：

$$f(i,j,1)=sum0(i-1,j-1)$$

$$f(i,j,0)=sum1(i-1,i-1)-sum1(i-1,j-1)$$

时间复杂度为 $O(n^2)$。

因为 $f$ 和 $sum$ 只用到了当前一层和上一层，所以只存这两层就好，空间复杂度为 $O(n)$。

# 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int mn=4207;
long long f[mn][2],sum0[2][mn],sum1[2][mn];
int main()
{
	int n,p;
	scanf("%d%d",&n,&p);
	f[1][1]=f[1][0]=sum0[1][1]=sum1[1][1]=1;
	for(int i=2;i<=n;++i)
	{
		int now=i&1;  //now为当前层，now^1为上一层。这里相当于 now=i%2 
		for(int j=1;j<=i;++j)
		{
			f[j][1]=sum0[now^1][j-1];
		  	f[j][0]=(sum1[now^1][i-1]-sum1[now^1][j-1])%p;
		  	while(f[j][0]<0)  f[j][0]+=p;  //避免取模后相减变为负数 
		  	sum0[now][j]=(sum0[now][j-1]+f[j][0])%p;
		  	sum1[now][j]=(sum1[now][j-1]+f[j][1])%p;
		}
	}
	  
	long long ans=0;
	for(int i=1;i<=n;++i)  ans=((ans+f[i][0])%p+f[i][1])%p;
	cout<<ans;
} 
```

