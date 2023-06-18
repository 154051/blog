---
author: 154051
avatar: 'https://cdn.jsdelivr.net/gh/154051/cdn@master/img/头像new.jpg'
title: 【题解】P3761 [TJOI2017]城市
tags:
  - 树
mathjax: true
date: 2021-08-08 20:31:36
categories: 题解
description: 证明了O(n)做法
keywords: P3761
photos: https://cdn.jsdelivr.net/gh/154051/cdn@master/img/8-8_2.jpg
---
[P3761 [TJOI2017]城市](https://www.luogu.com.cn/problem/P3761)

**前排提醒：本文着重证明了 $O(n)$ 的做法，$O(n^2)$ 的做法讲得不是很明白。**

## 题意

给一颗有边权的树，删去一条边并加上一条权值相同的边（要求操作后还是一颗树），求树的直径最小是多少。

$n\le 5000$。

## 题解

很妙的一道题，主要难点在于结论的寻找和证明。

最朴素的做法是枚举每条要删除的边，整棵树被分为了两颗子树，再枚举两颗子树上选的点，计算直径。时间复杂度 $O(n^4)$。

### $O(n^2)$ 做法

新加入一条边后，新树的直径有两种情况：

* 直径在分出来的两颗树中的一个。

* 直径经过了新加入的边。

对于第一种情况，新加入的边不会产生影响。

对于第二种情况，要想办法连边使得新树的直径最小。可以发现两棵树要连的点都是树的中心（即该点到其他节点的最远距离最小）。

考虑 $DP$，第一遍 $\text{dfs}$ 求出在该节点子树内从它出发的最远和次远距离。

第二遍 $\text{dfs}$ 根据父亲的信息求出在整棵树中从该节点出发的最远和次远距离，在这个过程中找出树的中心。

所以优化后的做法为枚举每条要删除的边，求出分出来的两颗树的直径和中心，比较这三个值，取最大。时间复杂度 $O(n^2)$。

这个复杂度已经可以通过这道题了，但是还能继续优化到 $O(n)$。

### $O(n)$ 做法


时间复杂度的瓶颈主要在 $DP$，考虑优化。

**结论一：树的中心是树的直径的（带权）中点。**

证明：

* 对于直径上任意一点 $u$，显然从 $u$ 出发的最远路径一定在直径上，若要求直径中心则只需比较 $u$ 到直径两端的最远距离，所以直径中点 $mid$ 是该直径的中心。

* 对于不在直径上的点 $v$，从 $v$ 出发到 $mid$，再到直径较远一端的所走路程，必然比直接从 $mid$ 出发的所走路程远。根据定义，树的中心要到其他点的最远距离最小，所以 $v$ 不是树的中心。

综上，$mid$ 是树的中心。证毕。

问题就转化为了如何求树的直径的中点。

对于删边操作，可以发现只有删原树直径上的边才有意义，因为删其他边直径没有变。

所以从直径一端出发到另一端依次删除边。但是每次都要遍历整棵树求直径中点，时间复杂度还是没有变，考虑优化。

以下只针对删边后的两颗树中的一个分析，另一个同理。

**结论二：对于一颗分割出来的树，它直径的一端一定是原树直径的一端。**

证明：

反证法，如图，

![](https://pic.imgdb.cn/item/610f4e4b5132923bf8ccee00.png)

删边后得到了以 $C$ 为根的子树，假设 $EF$ 该树的直径，说明 $\text{dis}(E,F)>\text{dis}(A,B)$，$\text{dis}(E,F)+\text{dis}(B,E)>\text{dis}(A,B)$，则 $FD$ 比 $AD$ 更长，与 $AD$ 是原树直径矛盾，该假设不成立，所以 $A$ 一定是删边后树的直径的一端。证毕。

考虑如何快速维护删边后树的直径：

（图片来自[getchar123的题解](https://www.luogu.com.cn/blog/getchar123/solution-p3761)）

![](https://pic.imgdb.cn/item/610f59395132923bf8db329e.png)

（橙色为截掉的边，绿色为直径，右图紫色的点为重复遍历的点，红色为新遍历的点）

在遍历删边的过程中，如果每次都遍历整棵树，会有大量重复节点（紫色点），根据结论二，我们只需要考虑经过新加进来的 原树直径上的点 能否得到更长的路径。

即从 $10$ 号点出发遍历红色点，找到最长路径，加上 $\text{dis}(1,10)$，与原来的直径比较。

这样每次都只需要遍历新加进来的节点，维护直径的总时间复杂度优化到了 $O(n)$。

但是每次维护直径中点都要遍历一遍直径，时间复杂度还是 $O(n^2)$，继续优化。

**结论三：对于分割出来的树，它的直径中点一定在原树直径上。**

证明：

如图，删边后的树的直径一定是形如这个样子，

![](https://pic.imgdb.cn/item/610f5e3d5132923bf8e19add.png)

分割出来的树的直径和原树的直径的重叠部分一定大于非重叠部分，即 $\text{dis}(A,B)>\text{dis}(E,B)$，如果不是，则路径 $ED$ 就会比原树直径还长。

根据直径中点的定义，中点一定在路径 $AB$ 上。证毕。

可以发现删边过程中，分割出来的树的直径是不下降的，新树和原树的重叠部分也是不下降的。

所以每次只需要从上一次的中点开始，在原树直径上向后遍历，直到当前点比上一个点劣时停止（再往后只会更劣）。维护中点的总时间复杂度为 $O(n)$。

综上，该算法的时间复杂度为 $O(n)$。

这里只维护了分割出来的其中一棵树，对于另一棵树可以倒过来重复这些操作。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int mn=5e3+7;
struct node {
    int mid,len_L,len_R;  //分别是中点，直径，半径（中点到其他点的最远距离）
}ans[3][mn];
int tt=0,fr[mn],nx[mn*2],to[mn*2],w[mn*2];
int rt=1,rt2=1,maxlen=0,d[mn],mx=0;
int q[mn],top=0;  //存直径上所有点
bool vis[mn];
void add(int x,int y,int v)
{
    ++tt;
    nx[tt]=fr[x];
    fr[x]=tt;
    to[tt]=y;
    w[tt]=v;
}
void findrt(int x,int fa,int dis)
{
    if(dis>maxlen)  maxlen=dis,rt=x;
    for(int i=fr[x];i;i=nx[i])
      if(to[i]!=fa)  findrt(to[i],x,dis+w[i]);
}
void findrt2(int x,int fa,int dis)
{
    d[x]=dis;
    if(dis>maxlen)  maxlen=dis,rt2=x;
    for(int i=fr[x];i;i=nx[i])
      if(to[i]!=fa)  findrt2(to[i],x,dis+w[i]);
}
void deep(int x,int fa,int dis)
{
    d[x]=dis;
    for(int i=fr[x];i;i=nx[i])
      if(to[i]!=fa)  deep(to[i],x,dis+w[i]);
}
bool dfs(int x,int fa,int dis)
{
    q[++top]=x;
    
    if(x==rt2)  return 1;
    for(int i=fr[x];i;i=nx[i])
    {
        int y=to[i];
        if(y==fa)  continue;
        bool flag=dfs(y,x,dis+w[i]);
        if(flag)  return 1;
    }
    --top;
    return 0;
}
void dfs2(int x,int dis)
{
    vis[x]=1;
    mx=max(mx,dis);
    for(int i=fr[x];i;i=nx[i])
    {
        int y=to[i];
        if(vis[y])  continue;
        dfs2(y,dis+w[i]);
    }
}
void sol(int k)
{
    memset(vis,0,sizeof(vis));
    for(int i=1;i<=top;++i)  vis[q[i]]=1;
    int mid=1,len_L=0,len_R=0;
    ans[k][1]=(node){mid,len_L,len_R};
    for(int i=2;i<top;++i)
    {
        mx=0;
        dfs2(q[i],0);  //遍历新加进来的节点
        if(d[q[i]]-d[q[1]]+mx<=len_L)
            ans[k][i]=(node){mid,len_L,len_R};
        else
        {
            len_L=d[q[i]]-d[q[1]]+mx;
            len_R=max(d[q[mid]]-d[q[1]],len_L-d[q[mid]]-d[q[1]]);
            for(int j=mid+1;j<=i;++j)
            {
                int dis=max(d[q[j]]-d[q[1]],len_L-d[q[j]]-d[q[1]]);
                if(dis>len_R)  break;  //这里就直接跳出，后面只会更劣
                len_R=dis;mid=j;
            }
            ans[k][i]=(node){mid,len_L,len_R};
        }
    }
}
int main()
{
    int n;
    cin>>n;
    for(int i=1;i<n;++i)
    {
        int x,y,v;
        scanf("%d%d%d",&x,&y,&v);
        add(x,y,v);
        add(y,x,v);
    }
    findrt(1,0,0);  //找直径一端
    maxlen=0;
    findrt2(rt,0,0);  //找另一端
    maxlen=0;
    dfs(rt,0,0);  //存直径
    sol(1);
/***************以下为倒过来处理另一棵树***************/
    memset(d,0,sizeof(d));
    top=0;
    swap(rt,rt2);
    deep(rt,0,0);  //根变了，深度也要重新求
    dfs(rt,0,0);
    sol(2);

    int ANS=1e9;
    for(int i=1;i<top;++i)
      ANS=min(ANS,max(d[q[i+1]]-d[q[i]]+ans[2][i].len_R+ans[1][top-i].len_R,max(ans[2][i].len_L,ans[1][top-i].len_L)));
    cout<<ANS;
    return 0;
}
```
