---
author: 154051
avatar: 'https://cdn.jsdelivr.net/gh/154051/cdn@master/img/头像new.jpg'
title: 【题解】P4248 [AHOI2013]差异
tags:
  - 字符串
  - 后缀自动机,SAM
mathjax: true
date: 2021-07-17 19:49:52
categories: 题解
description: 后缀自动机求最长公共后缀
keywords: 后缀自动机
photos: https://cdn.jsdelivr.net/gh/154051/cdn@master/img/7-17.jpg
---

## 题意

给一个长度为 $n$ 的字符串，求 

$$\sum\limits_{1\le i<j\le n} \operatorname{len}(suf_i)+\operatorname{len}(suf_j)-2\times \operatorname{lcp}(suf_i,suf_j)$$

其中，$\text{lcp}$ 表示最长公共前缀。

## 题解

后缀的最长公共前缀不太好求，我们把给的字符串翻转，再将原式变一下，就成了

$$\sum\limits_{1\le i<j\le n}\operatorname{len}(pre_i)+\operatorname{len}(pre_j)-\sum\limits_{1\le i<j\le n}2\times \operatorname{lcs}(pre_i,pre_j)$$

其中，$\operatorname{lcs}$ 表示最长公共后缀。

前一部分很好求，即 $n\times (n+1)\times (n-1)/2$。

后一部分可以对原串建 $SAM$，对于 $SAM$ 上的一个结点，它的祖先都是它的后缀，所以求最长公共后缀即在 $parent\ \ tree$ 上求这两个结点的 $LCA$。

但是 $n$ 太大，不能直接枚举 $i$ 和 $j$，考虑对每个 $parent\ \ tree$ 上的结点求该结点是多少个点对的 $LCA$，具体做法为每遍历一个儿子，就把该儿子的子树大小乘上前面已经遍历过的子树大小，再乘上 $longest$ 的二倍累加到答案中。时间复杂度为 $O(n)$。

其实题面上也有提示，原式很像树上求两点之间的路径长度，于是可以联想到 $parent\ \ tree$ 上求 $LCA$。
~~（谁会联想这个啊）~~

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const int mn=2e6+7;
struct node {
    int ch[26],fa;
    ll len;
}sam[mn];
int last=1,tot=1;
char tmp[mn],s[mn];
int fr[mn],nx[mn],to[mn],tt=0;
ll ans=0,cnt[mn];
void add(int x)
{
    int p=last,now=last=++tot;cnt[tot]=1;
    sam[now].len=sam[p].len+1;
    for(;p&&!sam[p].ch[x];p=sam[p].fa)  sam[p].ch[x]=now;
    if(!p)  sam[now].fa=1;
    else
    {
        int son1=sam[p].ch[x];
        if(sam[son1].len==sam[p].len+1)  sam[now].fa=son1;
        else
        {
            int son2=++tot;
            sam[son2]=sam[son1];
            sam[son2].len=sam[p].len+1;
            sam[son1].fa=sam[now].fa=son2;
            for(;p&&sam[p].ch[x]==son1;p=sam[p].fa)  sam[p].ch[x]=son2;
        }
    }
}
void link(int x,int y)
{
    ++tt;
    nx[tt]=fr[x];
    fr[x]=tt;
    to[tt]=y;
}
void dfs(int x)
{
    ll sum=0;
    for(int i=fr[x];i;i=nx[i])
    {
        int y=to[i];
        dfs(y);
        sum+=sam[x].len*cnt[x]*cnt[y]*2;  //这里不开 long long 会乘炸
        cnt[x]+=cnt[y];
    }
    ans-=sum;
}
int main()
{
    scanf("%s",tmp+1);
    int n=strlen(tmp+1);
    for(int i=1;i<=n;++i)
      s[n-i+1]=tmp[i];
    for(int i=1;i<=n;++i)  add(s[i]-'a');
    for(int i=2;i<=tot;++i)  link(sam[i].fa,i);
    ans=(ll)n*(n+1)*(n-1)/2;  //这里也会乘炸
    dfs(1);
    cout<<ans;
    return 0;
}
```