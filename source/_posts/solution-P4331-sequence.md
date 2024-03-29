---
author: a154051
avatar: 'https://a154051-img-1321592229.cos.ap-beijing.myqcloud.com/img/头像new.jpg'
title: 【题解】P4331 [BalticOI 2004]Sequence
tags:
  - 贪心
mathjax: true
date: 2021-09-25 16:35:02
categories: 题解
description: 贪心加数据结构
keywords: 贪心
photos: https://a154051-img-1321592229.cos.ap-beijing.myqcloud.com/img/9-25-2.jpg
---
[P4331 [BalticOI 2004]Sequence 数字序列](https://www.luogu.com.cn/problem/P4331)

好多题解都是用的中位数做法，其实贪心就可以直接做。

## 题意

给定一个长度为 $n$ 的整数序列 $a$，求出一个严格递增整数序列 $b$，使得 $\sum^n_{i=1}\left\vert a_i-b_i\right\vert$ 最小。

## 做法

首先给 $b$ 赋一个初值，使得 $b_i\le a_i(i\in n)$ 并且 $b_i=b_{i-1}+1(i\in [2,n])$。

在这种情况下，要想使答案更优只能增大 $b$ 序列或不变。

因为 $b$ 严格递增，并且 $b_i=b_{i-1}+1$，所以要想使 $b_i$ 增加 $x$，就要对 $[i,n]$ 整个区间加上 $x$。

定义 $val_i=\begin{cases}1&a_i>b_i \\ -1&a_i\le b_i\end{cases}$，$sum_i=\sum\limits_{j=i}^n val_j$。

然后重复执行下列操作：

1. 在 $[1,n]$ 中找到最大的 $sum$ 的位置 $k$，可以发现 $b_i=b_{i-1}+1(i\in[k+1,n])$，证明在下面。

2. 设 $x=\min\{a_i-b_i \}(i\in[k,n],a_i-b_i>0)$，令 $b_i+x(i\in[k,n])$。

3. 更新 $val$ 和 $sum$。

直到所有的 $sum$ 都小于等于 $0$。

正确性证明：

对初始序列 $b$ 执行一遍该操作显然正确。

记 $last$ 为上一次操作中最大的 $sum$ 的位置，$k$ 为这次操作中最大的 $sum$ 的位置。

可以得到，对于任意的 $i\in [1,last-1]$，都有 $sum_i\le sum_{last}$。

在上一次操作中，$[last,n]$ 中至少会有一个位置的 $val$ 变为 $-1$，则 $sum_i(i\in[1,last-1])$ 只会变得更小，所以 $k$ 不可能在 $[1,last-1]$ 中，$k$ 只会越来越靠右，因此 $b_i=b_{i-1}+1(i\in[k+1,n])$，所以对 $b_i(i\in [k,n])$ 执行该操作显然也正确。

证毕。

用线段树维护 $sum$，$set$ 维护 $[k,n]$ 中 $a_i-b_i(i\in[k,n])$ 大于 $0$ 的元素。 

每次操作用线段树求出 $[1,n]$ 中最大的 $sum$ 的位置 $k$，将 $set$ 中位置在 $[1,k-1]$ 的元素删除，在 $set$ 中查询此时 $a_i-b_i$ 的最小值，将等于该值的元素删除，并在线段树上将区间 $[1,\text{该元素位置}]$ 中所有 $sum$ 减 $1$。

由于不会添加元素，所以 $set$ 至多有 $n$ 次删除操作，线段树至多有 $n$ 次修改操作。

时间复杂度为 $O(n\log n)$。

复杂度虽然正确，但常数有点大，时间卡得很紧，要开 $O2$ 才能过。

## 代码

```cpp
#include <bits/stdc++.h>
#define mk(x,y) make_pair(x,y)
#define ls now<<1
#define rs now<<1|1
using namespace std;
typedef long long ll;
const int mn=1e6+7;
struct tree {
    int ans,mx,tag;
}tr[mn<<2];
int a[mn],sum[mn],val[mn];
ll b[mn];
set<pair<ll,int> > st;
int in()
{
    int x=0;
    char c=getchar();
    while(!isdigit(c))  c=getchar();
    while(isdigit(c))
    {
        x=x*10+c-'0';
        c=getchar();
    }
    return x;
}
void upd(int now)
{
    if(tr[rs].mx>=tr[ls].mx)
      tr[now].mx=tr[rs].mx,tr[now].ans=tr[rs].ans;
    else
      tr[now].mx=tr[ls].mx,tr[now].ans=tr[ls].ans;
}
void build(int now,int l,int r)
{
    if(l==r)
    {
        tr[now].mx=sum[l];
        tr[now].ans=l;
        return ;
    }
    int mid=(l+r)>>1;
    build(now<<1,l,mid);
    build(now<<1|1,mid+1,r);
    upd(now);
}
void push(int now)
{
    if(!tr[now].tag)  return ;
    tr[now<<1].mx+=tr[now].tag;
    tr[now<<1].tag+=tr[now].tag;

    tr[now<<1|1].mx+=tr[now].tag;
    tr[now<<1|1].tag+=tr[now].tag;
    tr[now].tag=0;
}
void add(int now,int l,int r,int x,int v)
{
    if(r<=x)
    {
        tr[now].mx+=v;
        tr[now].tag+=v;
        return ;
    }
    push(now);
    int mid=(l+r)>>1;
    if(x>mid)  add(now<<1|1,mid+1,r,x,v);
    add(now<<1,l,mid,x,v);
    upd(now);
}

int main()
{
    int n,mi=1;
    n=in();
    ll ans=0;
    for(int i=1;i<=n;++i)
    {
        a[i]=in();
        mi=min(mi,a[i]-i+1);
    }
    for(int i=1;i<=n;++i)
    {
    	b[i]=mi+i-1;
    	ans+=abs(b[i]-a[i]);
    	if(b[i]<a[i])  val[i]=1,st.insert(mk(a[i]-b[i],i));
    	else  val[i]=-1;
	}
    sum[n]=val[n];
    for(int i=n-1;i>=1;--i)  sum[i]=sum[i+1]+val[i];
    build(1,1,n);
    int last=1,cnt=0;
    while(1)
    {
        if(tr[1].mx<=0){  //终止条件
            for(int i=last;i<=n;++i)  b[i]+=cnt;
            break;
        }
        int now=tr[1].ans;  //最大的 sum 的位置
        for(int i=last;i<now;++i){  //删除 set 中多余的元素
            b[i]+=cnt;
            if(a[i]-b[i]-cnt>0)  st.erase(mk(a[i]-b[i],i));
        }
        last=now;
        ans-=tr[1].mx*((*st.begin()).first-cnt);
        cnt=(*st.begin()).first;
        while(st.size())  //删除 a-b 等于 0 的元素
        {
            pair<ll,int> it=*st.begin();
            if(it.first!=cnt)  break;
            add(1,1,n,it.second,-2);  //维护 sum
            st.erase(st.begin());
        }
    }
    printf("%lld\n",ans);
    for(int i=1;i<=n;++i)  printf("%lld ",b[i]);
    return 0;
}
```
