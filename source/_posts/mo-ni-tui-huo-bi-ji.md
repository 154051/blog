---
title: 【笔记】模拟退火算法 
author: a154051
avatar: https://a154051-img-1321592229.cos.ap-beijing.myqcloud.com/img/头像new.jpg
categories: 笔记
date: 2021-02-08 19:40:00
mathjax: true
tags: 
 - 模拟退火
keywords: 模拟退火笔记
description: 模拟退火笔记
photos: https://a154051-img-1321592229.cos.ap-beijing.myqcloud.com/img/鬼灭1new.jpg
---

# 简介

模拟退火(Simulated Annealing，SA)是一种随机化算法。它适用于解决方案量极大的问题，如旅行商问题，求解多峰函数极值等问题。

# 原理

> 模拟退火算法来源于固体退火原理，是一种基于概率的算法，将固体加温至充分高，再让其徐徐冷却，加温时，固体内部粒子随温升变为无序状，内能增大，而徐徐冷却时粒子渐趋有序，在每个温度都达到平衡态，最后在常温时达到基态，内能减为最小。
>
> ——百度百科

把问题看作固体内部粒子，把最优解看作平衡态，用程序模拟退火的过程，就是模拟退火算法。

# 过程

模拟退火中有三个参数，分别是初始温度 $T_0$、降温系数 $\Delta$。其中，$\Delta$ 是一个略小于 $1$ 的数。

令温度 $T=T_0$，然后每次降温时 $T=T*\Delta$，直到 $T=0$。

在每次降温过程中，都要用当前解产生一个新解，即 $x_1=x_0+\Delta x$。其中，$\Delta x \in R$ 并且 $\left\vert \Delta x\right\vert \propto T$，表示解的变动值，温度越低，它的变化幅度越小，产生的新解也会越来越趋近于最优解，如图，

![模拟退火过程](https://oi-wiki.org/misc/images/simulated-annealing.gif)

这时候我们需要考虑是否用新解替换当前解，定义当前温度为 $T$，某个解 $x$ 的贡献为 $f(x)$（这里的贡献不是指大小，而是这个解的优劣），则修改当前解的概率为 

$$P=\begin{cases}1&f(x_1)\geqslant f(x_0)\\e^\frac{-\left\vert f(x_1)-f(x_0)\right\vert}{T}&f(x_1)<f(x_0)\end{cases}$$

新解更优时，就一定替换。

新解更劣时，就以一定概率接受它，这样就不会停留在局部最优解，这个概率与两个因素有关（可以结合这两个因素再体会一下上面的式子）：

1. 当前温度。温度越高，接受劣解的概率越大，因为温度越高内部粒子越活跃。

2. 新解和当前解的贡献差。新解的贡献和当前解的贡献相差越大，接受的概率越小。

当温度降为 $0$ 时，就得到了最优解。

**注意**：为了使结果更准确，一般不会直接选取最后的解作为答案，而是在模拟退火的过程中记录遇到的所有解中的最优解。

# 代码

以[P1337 [JSOI2004]平衡点 / 吊打XXX](https://www.luogu.com.cn/problem/P1337)为例。

由~~题解~~物理知识可知，在平衡态时，系统的总能量最小，在该题中，总能量即物体总势能，物体离地面高度越低，势能越小，也就是使绳子在桌子上的长度尽量小。

所以物体的势能与桌子上绳子的长度和物体的质量成正比，也就是找一点使得 $\sum_{i=1}^n dis_i * w_i$最小（$dis_i$ 是绳结到第 $i$ 个洞口的距离）。

代码如下
```cpp
#include <bits/stdc++.h>
#define rd t*(rand()*2-RAND_MAX)     
//rand()的范围是 [0,RAND_MAX)。RAND_MAX是一个常量，表示rand()的最大值。
//这样写是为了使rand()能随机到负数。 
using namespace std;
const long double eps=1e-13,down=0.993;
const int mn=1e3+7;
int n;
double x[mn],y[mn],w[mn];
long double best_x,best_y,minn;  //全局最优解 
long double fx(long double x0,long double y0)   //计算贡献 
{
	long double rs=0,dx,dy;
	for(int i=1;i<=n;++i)
	{
		dx=x[i]-x0;dy=y[i]-y0;
		rs+=sqrt(dx*dx+dy*dy)*w[i];
	}
	return rs;
}
int main()
{
	srand(154051);  //随机数种子，不要也行。
	scanf("%d",&n);
	for(int i=1;i<=n;++i)
	{
		scanf("%lf%lf%lf",&x[i],&y[i],&w[i]);
		best_x+=x[i];best_y+=y[i];
	}
	long double ans;
	best_x/=n;best_y/=n;
	long double x0=best_x,y0=best_y;
	//得到一组初始解 
	minn=ans=fx(best_x,best_y);
	for(long double t=7000;t>eps;t*=down)
	{
		long double x1=x0+rd,y1=y0+rd;  //产生新解 
		long double rs=fx(x1,y1);  //计算新解的贡献 
		if(minn>rs)  //维护全局最优解 
		{
			minn=rs;
			best_x=x1;best_y=y1;
		}
		if(rs<ans||exp((ans-rs)/t)*RAND_MAX>rand())  //exp()是c++库函数，exp(x)是求e的x次方 
		//相当于exp((ans-rs)/t)>(double)rand()/RAND_MAX，(double)rand()/RAND_MAX可以得到一个0~1之间的小数。一般会把除法变乘法保证精度。 
		{
			ans=rs;
			x0=x1;y0=y1;
		}
	}
	printf("%.3Lf %.3Lf",best_x,best_y);
	return 0;
}
```
