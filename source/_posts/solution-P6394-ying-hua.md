---
title: 【题解】P6394樱花，还有你
author: 154051
avatar: https://cdn.jsdelivr.net/gh/154051/cdn@master/img/头像new.jpg
categories: 题解
date: 2021-02-27 11:14:00
mathjax: true
tags: 
 - DP
keywords: 题解P6394樱花，还有你
description: 基础DP优化好题
photos: https://cdn.jsdelivr.net/gh/154051/cdn@master/img/SakuraAndYou.jpg
---
[P6394 樱花，还有你](https://www.luogu.com.cn/problem/P6394)

题面好浪漫，~~可惜我是11月11日写的这题~~。

# 题意

有 $k$ 棵树，每棵树上有 $s_i$ 朵花，有几种方案能恰好取 $n$ 朵花，若收集不到 $n$ 朵花，输出**impossible**。

**注意**：用 $(a_1,a_2,⋯)$ 表示一种方案，则 $(1,1,1)$ 和 $(1,1,1,0)$ 是两种不同的方案。

# 分析
## Subtask 1

$\sum s_i<n$

白给，直接输出**impossible**即可。

## Subtask 2&3

$n,k \le 5\times10^2$

定义 $f(i,j)$ 表示前 $i$ 棵树收集 $j$ 朵花的方案数。

显然有状态转移方程：

$$f(i,j)=\sum\limits_{x=0}^{\min(j,s_i)}f(i-1,j-x)$$

最终答案为 $\sum_{i=1}^{k}f(i,n)$。

时间复杂度：$O(n^2k)$，空间复杂度：$O(nk)$。

## Subtask 4

$n,k \le 5\times10^3$

$O(n^2k)$ 的复杂度会超时，需要优化。

发现可以用前缀和优化掉最里面一层循环。

定义 $sum(i,j)=\sum_{x=0}^j f(i,x)$。

所以

$$f(i,j)=sum(i-1,j)-sum(i-1,j-\min(j,s_i)-1)$$

为了避免数组下标为负数，应改为

$$f(i,j)=sum(i-1,j)-sum(i-1,j-\min(j,s_i))+f(i-1,j-\min(j,s_i)$$

时间复杂度：$O(nk)$。

这时候发现~~无耻~~出题人只给了 $64MB$，内存会炸。

观察上述转移方程，发现 $f$ 数组和 $sum$ 数组只用到了这一层和上一层，所以只存这两层就好。

空间复杂度：$O(n)$。

# 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int mn=5e3+7,mod=10086001;
int s[mn],f[2][mn],sum[2][mn];
int now;  //now为当前这一层，now^1为上一层 
int min1(int x,int y)
{
	return x<y?x:y;
}
int main()
{
	int n,k,cnt=0;
	cin>>n>>k;
	f[0][0]=sum[0][0]=f[1][0]=sum[1][0]=1;
	for(int i=1;i<=k;++i)
	  scanf("%d",&s[i]),cnt+=s[i];
	if(cnt<n)
	{
		cout<<"impossible";
		return 0;
	}
	int ans=0;
	for(int i=1;i<=k;++i)
	{
		now=i&1;  //相当于now=i%2 
		for(int j=1;j<=n;++j)
		{
			sum[now^1][j]=(sum[now^1][j-1]+f[now^1][j])%mod;
			f[now][j]=(sum[now^1][j]-sum[now^1][j-min1(j,s[i])]+f[now^1][j-min1(j,s[i])])%mod;
			while(f[now][j]<0)  f[now][j]+=mod;  //取模后相减有可能出现负数 
		}
		ans=(ans+f[now][n])%mod;
	}
	cout<<ans;
	return 0;
}
```
