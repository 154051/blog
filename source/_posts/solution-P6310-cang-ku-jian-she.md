---
author: a154051
avatar: 'https://cdn.jsdelivr.net/gh/154051/cdn@master/img/头像new.jpg'
title: 【题解】P6310 [Wdsr-1]仓库建设
tags:
  - Kruskal重构树
mathjax: true
date: 2023-09-25 23:17:51
categories: 题解
description: Kruskal重构树好题
keywords: Kruskal重构树
photos: https://pic.imgdb.cn/item/651263edc458853aefbb11d1.jpg
---

[P6310 「Wdsr-1」仓库建设 ](https://www.luogu.com.cn/problem/P6310)

两年没写题解了，本来想找个蓝题练练手，结果被狠狠薄纱了，硬刚了半天才写出来。变量名极为抽象，写到后面我自己都搞混了。

## 题意

给定一张无向联通图，边有边权，可以选择一些城市发出粮车，第 $i$ 座城市的粮车的油量可以行驶的路程为 $x_i$，每到一个城市都会加满油，第一问为求最少选择几个城市可以覆盖所有城市，第二问为当第 $i$ 个城市无法选择时最少选择多少城市才能满足条件，如果不能则输出 $-1$。

## 题解

显然我们希望路径中最大的边权最小，自然想到了 Kruskal 重构树，重构树的叶子节点为原图的 $n$ 个节点，其他节点代表原图的一条边，点权即是边权，深度越浅的点点权越大。

设 $val$ 表示点权，从某个叶子节点 $i$ 向上找到最浅的节点 $j$ 满足 $val_j \le x_i$，记为 $fa_i$，如果选择了 $i$，则以 $fa_i$ 为根的子树的叶子节点都可以被覆盖。显然如果某条路径上有多个 $fa_i$ 我们要选择深度最浅的那个，则该子树内的其他点都不用选。

所以第一问只需要从根节点向下遍历，遇到 $fa$ 节点就将计数器加一，并直接返回，不用遍历该子树。

对于第二问，假设当前无法选择的节点为 $i$，如果 $fa_i$ 不是第一问选择的节点，则答案不变。

如果从 $i$ 到根的路径上只有 $fa_i$，若 $\exists j \neq i,fa_j = fa_i$ 则答案不变，若这样的 $j$ 不存在，则无法到达 $i$，答案为 $-1$。

如果 $fa_i$ 是第一问选择的节点且从 $i$ 到根的路径上还有其他的 $fa$，则只需要在 $fa_i$ 子树内选择深度最浅的几个 $fa$ 节点就能覆盖所有点，像第一问一样在 $fa_i$ 的子树内遍历，遇到 $fa$ 节点就将计数器加一，并直接返回，不用遍历该子树，最后将第一问的答案减一再加上计数器就是最终答案。

向上找 $fa$ 用倍增实现，其他值都可以一次 dfs 求出来，总时间复杂度为 $O(n \log n)$。

## 代码

```
#include <bits/stdc++.h>
using namespace std;
const int N=3e5+7;
struct edge {
    int u,v,w;
}e[N];
int a[N],fa[N],val[N<<1],f[N<<1][21],dep[N<<1],cnt[N<<1],num[N],leaf[N<<1],up[N<<1];
//cnt是统计某个fa节点的子树内有多少个深度最浅的fa节点，num是从叶子节点到根有多少fa节点
//leaf是某个fa节点对应的叶子节点有几个，up是距离fa节点最近的祖先fa节点
bool vis[N<<1];
vector<int> son[N<<1];
bool cmp(edge x,edge y){return x.w<y.w;}
int fd(int x) {
    if(!fa[x])  return x;
    return fa[x]=fd(fa[x]);
}
void dfs1(int x,int FA)
{
    dep[x]=dep[FA]+1;f[x][0]=FA;
    for(int i=0;i<son[x].size();++i)  dfs1(son[x][i],x);
}
void dfs2(int x,int FA,int CNT)
{
    if(vis[x])  ++cnt[FA],++CNT,up[x]=FA;
    for(int i=0;i<son[x].size();++i) {
        if(vis[x])  dfs2(son[x][i],x,CNT);
        else  dfs2(son[x][i],FA,CNT);
    }
    if(!son[x].size())  num[x]=CNT;
}
int main()
{
    int n,m;
    cin>>n>>m;
    int tot=n;
    for(int i=1;i<=m;++i)  scanf("%d%d%d",&e[i].u,&e[i].v,&e[i].w);
    sort(e+1,e+1+m,cmp);
    for(int i=1;i<=m;++i) {
        int x=fd(e[i].u),y=fd(e[i].v);
        if(x!=y) {
            fa[x]=fa[y]=++tot;
            son[tot].push_back(x);
            son[tot].push_back(y);
            val[tot]=e[i].w;
        }
    }
    for(int i=1;i<=n;++i)  scanf("%d",&a[i]);
    dfs1(tot,0);
    for(int j=1;j<=20;++j)
        for(int i=1;i<=tot;++i)
            f[i][j]=f[f[i][j-1]][j-1];
    for(int i=1;i<=n;++i) {
        int now=i;
        for(int j=20;j>=0;--j)
            if(f[now][j]&&a[i]>=val[f[now][j]])  now=f[now][j];
        vis[now]=1;
        fa[i]=now;
        ++leaf[now];
    }
    dfs2(tot,0,0);
    printf("%d ",cnt[0]);
    for(int i=1;i<=n;++i) {
        if(num[i]==1&&leaf[fa[i]]<=1)  printf("-1 ");
        else if(up[fa[i]]||leaf[fa[i]]>1)  printf("%d ",cnt[0]);
        else printf("%d ",cnt[0]-1+cnt[fa[i]]);
    }
    return 0;
}
```

