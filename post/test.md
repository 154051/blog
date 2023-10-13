---
title: test
author: 154051
avatar: https://a154051-img-1321592229.cos.ap-beijing.myqcloud.com/img/头像new.jpg
categories: 测试
date: 2020-12-20 16:40:00
mathjax: true
tags: 
 - code
 - latex
keywords: test
description: 总之就是测试各种东西
photos: https://a154051-img-1321592229.cos.ap-beijing.myqcloud.com/img/鬼灭1new.jpg
top: true
---

{% spoiler "隐藏内容的标题" %}

`表情示例（喝水）{% emoji_coda 2233/heshui %}`

表情示例（喝水）{% emoji_coda 2233/heshui %}



$$lim_{1\to+\infty}P(|\frac{1}{n}\sum_i^nX_i-\mu|<\epsilon)=1, i=1,...,n$$

```cpp
#include <bits/stdc++.h>
using namespace std;
const int mn=1e6+3;
int v[mn],a[mn][2],m[mn],ans=-1;
int f(int k)
{
	m[k]=1;
	if(a[k][0]==-1&&a[k][1]==-1)
	  return m[k]=1;
	if(a[k][0]!=-1)
	  m[k]+=f(a[k][0]);
	if(a[k][1]!=-1)
	  m[k]+=f(a[k][1]);
	return m[k];
}
bool p2(int x,int y)
{
	if(a[x][0]==-1&&a[x][1]==-1&&a[y][0]==-1&&a[y][1]==-1)
	  return true;
	if((a[x][0]==-1&&a[y][1]!=-1)||(a[x][0]!=-1&&a[y][1]==-1)||(a[x][1]==-1&&a[y][0]!=-1)||(a[x][1]!=-1&&a[y][0]==-1))
	  return false;
	if(a[x][0]!=-1&&a[y][1]!=-1)
	{
		if(m[a[x][0]]!=m[a[y][1]]||v[a[x][0]]!=v[a[y][1]])
		  return false;
		if(p2(a[x][0],a[y][1])==false)
		  return false;
	}
	if(a[x][1]!=-1&&a[y][0]!=-1)
	{
		if(m[a[x][1]]!=m[a[y][0]]||v[a[x][1]]!=v[a[y][0]])
		  return false;
		if(p2(a[x][1],a[y][0])==false)
		  return false;
	}
	return true;
}
bool p(int k)
{
	if(a[k][0]==-1&&a[k][1]==-1)
	  return true;
	if(a[k][0]==-1||a[k][1]==-1)
	  return false;
	if(m[a[k][0]]!=m[a[k][1]]||v[a[k][0]]!=v[a[k][1]])
	  return false;
	return p2(a[k][0],a[k][1]);
}
void dfs(int k)
{
	if(p(k)==true)
	{
		ans=max(ans,m[k]);
		return ;
	}
	if(a[k][0]!=-1)
	  dfs(a[k][0]);
	if(a[k][1]!=-1)
	  dfs(a[k][1]);
	return ;
}
int main()
{
	int n;
	cin>>n;
	for(int i=1;i<=n;++i)
	  scanf("%d",&v[i]);
	for(int i=1;i<=n;++i)
	  scanf("%d%d",&a[i][0],&a[i][1]);
	f(1);
	dfs(1);
	cout<<ans;
	return 0;	
} 

```

{% endspoiler %}