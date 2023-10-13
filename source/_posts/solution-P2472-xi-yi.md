---
author: a154051
avatar: 'https://a154051-img-1321592229.cos.ap-beijing.myqcloud.com/img/头像new.jpg'
title: 【题解】P2472 [SCOI2007]蜥蜴
mathjax: true
date: 2021-03-05 16:36:55
tags: 
 - 网络流
categories: 题解
description: 网络瘤，毒瘤的瘤。
photos: https://a154051-img-1321592229.cos.ap-beijing.myqcloud.com/img/P2472.jpg
---

[P2472 [SCOI2007]蜥蜴](https://www.luogu.com.cn/problem/P2472)

**网络瘤，毒瘤的瘤。**

蒟蒻第一次不看题解写出来网络瘤（~~然而还是逃不过debug半天的命运~~）。{% emoji_coda quyin/hematemesis %}

# 题意

在 $r\times c$ 的网格中，每个格子都有高度，高度为 $0$ 则不存在，一些格子上有蜥蜴，蜥蜴可以跳到直线距离不超过 $d$ 的格子上，每跳一次，原来的格子高度就会减一，求最少有几只蜥蜴不能逃离。

# 分析

显然，每个格子跳了一定次数后就不能再跳了，那么如何在网络流中加入这个限制呢？

假设原来的点编号为 $i$，高度为 $h$，总共 $n$ 个点，我们可以把这个点拆成两个，编号为 $i$ 和 $i+n$，$i$ 负责原来这个点的入边，$i+n$ 负责出边，再连一条从 $i$ 到 $i+n$ 容量为 $h$ 的边，如图：

![P2472](https://img.imgdb.cn/item/6041e56b360785be54ee87f8.png)

这个思想类似于[最小割点问题](https://www.luogu.com.cn/problem/P1345)。

# 建图

**注：以下说的点都是高度大于 $0$ 的点，高度为 $0$ 的点直接忽略。**

- 对于每个点都将其拆为入点和出点，容量为格子高度。

- 对于能跳出网格的点，从它的出点到汇点连一条容量无穷大的边。

- 对于有蜥蜴的点，从源点到它的入点连一条容量为 $1$ 的边（因为每个点上最多有 $1$ 个蜥蜴）。

其中，源点和汇点都是原图中不存在的，需要另外新建。

最后跑最大流，得到最多能逃离的蜥蜴数，用蜥蜴总数减去最大流即为所求答案。

# 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int mn=407,inf=1e7;
struct node{
	int x,y;
};
vector<node> p;
int n,m,d,tot=0,num[27][27],q[2*mn],dep[2*mn];
int fr[2*mn],fr2[2*mn],nx[100*mn],to[100*mn],val[100*mn],tt=1;
void add(int x,int y,int w)
{
	++tt;
	nx[tt]=fr[x];
	fr[x]=tt;
	to[tt]=y;
	val[tt]=w;
}
bool bfs()
{
	for(int i=0;i<=n*m*2+1;++i)  dep[i]=0,fr2[i]=fr[i];  //0 为源点，2*n*m+1 为汇点 
	int h=1,t=1;
	q[1]=0;dep[0]=1;
	while(h<=t)
	{
		int u=q[h];
		for(int i=fr[u];i;i=nx[i])
		{
			int v=to[i];
			if(dep[v]||!val[i])  continue;
			q[++t]=v;
			dep[v]=dep[u]+1;
			if(v==n*m*2+1)  return 1;
		}
		++h;
	}
	return 0;
}
int dfs(int u,int in)
{
	if(u==n*m*2+1)  return in;
	int out=0;
	for(int &i=fr2[u];i;i=nx[i])  //当前弧优化，i 是引用，当 i 变化时 fr2 也会跟着变化 
	{
		int v=to[i];
		if(dep[u]==dep[v]-1&&val[i])
		{
			int rs=dfs(v,val[i]<in?val[i]:in);
			val[i]-=rs;val[i^1]+=rs;
			out+=rs;in-=rs;
			if(!in)  break;
		}
	}
	if(out==0)  dep[u]=0;
	return out;
}
inline int dist(node v0,node v1){return (v0.x-v1.x)*(v0.x-v1.x)+(v0.y-v1.y)*(v0.y-v1.y);}  //求两点的直线距离 
int main()
{
	scanf("%d%d%d",&n,&m,&d);
	for(int i=1;i<=n;++i)
	  for(int j=1;j<=m;++j)
	  {
	  	num[i][j]=++tot;
	  	int x;
	  	scanf("%1d",&x);
	  	if(x==0)  continue;
	  	add(num[i][j],num[i][j]+n*m,x);  //拆点 
	  	add(num[i][j]+n*m,num[i][j],0);
	  	p.push_back((node){i,j});
	  	if(i-d<1||j-d<1||i+d>n||j+d>m)  add(num[i][j]+n*m,2*n*m+1,inf),add(2*n*m+1,num[i][j]+n*m,0);  //连汇点 
	  }
	for(int i=0;i<p.size()-1;++i)  //建图 
	  for(int j=i+1;j<p.size();++j)
	  {
	  	if(dist(p[i],p[j])>d*d)  continue;
	  	node v0=p[i],v1=p[j];
	  	add(num[v0.x][v0.y]+n*m,num[v1.x][v1.y],inf);
	  	add(num[v1.x][v1.y],num[v0.x][v0.y]+n*m,0);
	  	add(num[v1.x][v1.y]+n*m,num[v0.x][v0.y],inf);
	  	add(num[v0.x][v0.y],num[v1.x][v1.y]+n*m,0);
	  }
	int cnt=0;
	for(int i=1;i<=n;++i)
	  for(int j=1;j<=m;++j)
	  {
	  	char ch=getchar();
	  	while(ch!='.'&&ch!='L')  ch=getchar();
	  	if(ch=='L')  ++cnt,add(0,num[i][j],1),add(num[i][j],0,0);  //连源点 
	  }
	int ans=0;
	while(bfs())  ans+=dfs(0,inf);
	cout<<cnt-ans;
	return 0;
}
```

