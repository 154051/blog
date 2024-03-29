---
author: a154051
avatar: 'https://a154051-img-1321592229.cos.ap-beijing.myqcloud.com/img/头像new.jpg'
title: 【题解】P3291 [SCOI2016]妖怪
tags:
  - 模拟退火
mathjax: true
date: 2021-09-01 20:11:16
categories: 题解
description: 玄学又好用的模拟退火
keywords: 
  - P3291
  - SCOI2016
photos: https://a154051-img-1321592229.cos.ap-beijing.myqcloud.com/img/9-1.jpg
---

蒟蒻不会凸包，于是用**模拟退火**过了这道题。

如果你还不会模拟退火，可以看看我之前写的[学习笔记](https://a154051.gitee.io/2021/02/08/mo-ni-tui-huo-bi-ji/)。

## 题意

来自[辰星凌的题解](https://www.luogu.com.cn/blog/ChenXingLing/solution-p3291)：

> 给出 $n$ 个点 $(x_i,y_i)$，设 $f_i(a,b)=x_i+y_i+\frac{bx_i}{a}+\frac{ay_i}{b}$，求出一组 $(a,b)$，使得 $\max\{f_{i\in[1,n]}(a,b)\}$ 最小，输出这个最小值。

## 做法

正常模拟退火流程，每次随机生成两个数 $(a,b)$，暴力遍历一遍所有点计算出当前的答案，与上一次的答案比较。

蒟蒻一开始只是想骗点分，没想到拿了 $80$ 分，于是就进入了痛苦的调参过程。（注：没有在洛谷提交，洛谷数据不全。）

最初提交时总是第 $7$ 个点错，后来有一次分比较低，但是第 $7$ 个点对了，我比较了一下认为是初始温度的问题，初始温度低会导致生成出来的数的范围小，而那个点最优情况下的 $(a,b)$ 大于这个范围，所以就一直错。

于是稍微修改一下参数就 $A$ 了，最后一共提交了 $172$ 次...

不开 $O2$ 会 $T$，时间复杂度 $O(\text{玄学})$。

## 代码

```cpp
#include <bits/stdc++.h>
#define rd t*(rand()*2-RAND_MAX)
#define max(x,y) (x<y?y:x)  //这里是个小优化，会快一点
using namespace std;
typedef long long ll;
const double eps=1e-6,down=0.89;
const int mn=1e6+7;
int n;
double x[mn],y[mn];
double sol(double a,double b)  
{
    double rs=0;
    for(int i=1;i<=n;++i)
    {
        double tmp=x[i]+y[i]+x[i]/a*b+y[i]/b*a;
        rs=max(rs,tmp);
    }
    return rs;
}
int main()
{
    srand(154051);
    scanf("%d",&n);
    for(int i=1;i<=n;++i)  scanf("%lf%lf",&x[i],&y[i]);
    ll a=1,b=1;
    double ans,minn;
    minn=ans=sol(1,1);
    for(double t=1100000;t>eps;t*=down)
    {
        ll aa=a+rd,bb=b+rd;  
        if(aa==0)  aa++;
        if(bb==0)  bb++;
        if(aa<0)  aa=-aa;
        if(bb<0)  bb=-bb;
        double rs=sol(aa,bb);  
        if(minn>rs)  minn=rs;
        if(rs<ans||exp((ans-rs)/t)*RAND_MAX>rand())  
        {
            ans=rs;
            a=aa;b=bb;
        }
    }
    printf("%.4f",minn);
    return 0;
}
```
