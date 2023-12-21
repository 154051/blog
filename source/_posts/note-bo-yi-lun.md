---
author: a154051
avatar: 'https://a154051-img-1321592229.cos.ap-beijing.myqcloud.com/img/头像new.jpg'
title: 【笔记】博弈论
tags:
  - 博弈论
mathjax: true
date: 2023-12-20 22:30:00
categories: 笔记
description: 蒟蒻的博弈论学习笔记
keywords: 博弈论
photos: https://a154051-img-1321592229.cos.ap-beijing.myqcloud.com/img/23-12-20.jpg
---


> 「 <font color=red>我正是来访者，六轩岛上的第 18 个人类！！！</font>」
> 
> 「不好意思，<font color=red>就算迎来了汝，也是 17 人。</font>」
>
>  <p align="right">—— 海猫鸣泣之时</p>



博弈论主要分为公平组合游戏、非公平组合游戏、反常游戏。本文将讲解公平组合游戏。

## 公平组合游戏

公平组合游戏（Impartial Game）的定义如下：

- 游戏有两个人参与，二者轮流做出决策，双方均知道游戏的完整信息；

- 任意一个游戏者在某一确定状态可以作出的决策集合只与当前的状态有关，而与游戏者无关；

- 游戏中的同一个状态不可能多次抵达，游戏以玩家无法行动为结束，且游戏一定会在有限步后以非平局结束。

有向图游戏是一个经典的公平组合游戏，它的定义如下：

在一个有向无环图中，只有一个起点，上面有一个棋子，两个玩家轮流沿着有向边推动棋子，不能走的玩家判负。

事实上，大部分公平组合游戏都可以转化为有向图游戏，将每一个游戏状态作为一个节点，若一个状态 $A$ 能够通过一次决策变为状态 $B$，则连边 $A\to B$，称 $B$ 为 $A$ 的后继状态。下面都在有向图游戏的基础上讨论，以下的讨论都假设先后手都采取最优策略。

定义 **必胜状态** 为 **先手**在该状态下能够通过一系列决策获得胜利（即先手必胜）；**必败状态** 为 在该状态下无论**先手**怎么决策，**后手**都能够通过一系列决策获得胜利（即后手必胜）。

通过推理，我们可以得出下面三条定理：

- 定理 1：没有后继状态的状态是必败状态。

- 定理 2：一个状态是必胜状态当且仅当存在至少一个必败状态为它的后继状态。

- 定理 3：一个状态是必败状态当且仅当它的所有后继状态均为必胜状态。

## SG 函数

为了更准确地描述胜败状态，我们引入 SG 函数。

定义 $\operatorname{mex}$ 函数为不属于集合 $S$ 的最小非负整数：

$$\operatorname{mex}\{S\}=\min\{x\}\quad (x\notin S,x\in N)$$

例如 $\operatorname{mex}\{ 0,1,2,5,7 \}=3$。

设状态 $x$ 有 $k$ 个后继状态 $y_1,y_2,\dots,y_k$，则有：

$$SG(x)=\operatorname{mex}\{SG(y_1),SG(y_2),\dots,SG(y_k)\}$$

显然，一个状态为必败状态的充要条件为该状态的 SG 值为 $0$，必胜状态的充要条件为 SG 值不为 $0$。

假如只有一个游戏，SG 函数好像没什么用，因为我们只需要知道 SG 值是否为 $0$，而不需要知道具体数值，但如果有多个游戏，情况就不一样了。

考虑由两个有向图游戏组成的组合游戏，其中，这两个游戏相互独立，即其中任意一个游戏的任意决策都不会影响其他游戏。这个组合游戏的规则是每次要选择一个有向图游戏进行一次移动，最终无法移动的玩家失败。

定义一个有向图游戏的 SG 值为起点的 SG 值。设这两个游戏的起点分别为 $A,B$。下面我们先假设 SG 值只能降低，详细解释稍后再说。

显然当这两个游戏的 SG 值都为 $0$ 时为必败状态。

若 $SG(A)=0,SG(B)\ne 0$，先手可以将 $B$ 移动到 SG 值为 $0$ 的后继状态（因为根据 SG 函数的定义，$B$ 的所有后继状态的 SG 集合包含所有比 $SG(B)$ 小的非负整数），此时为必败状态，因此原状态为必胜状态。

若 $SG(A)=SG(B)\ne 0$，先手降低任意一个游戏的 SG 值，后手总是可以将另一个游戏的 SG 值降低到与之相等，直到两个游戏的 SG 值为 $0$，此时这个状态由先手拿到，因此后手必胜，原状态为必败状态。

若 $SG(A)\ne SG(B),SG(A)\ne 0,SG(B)\ne 0$，先手只需将 SG 值较大的游戏降低到与另一个游戏相等，就回到了上面的状态，因此原状态为必胜状态。

在上面的讨论中，我们总是让 SG 值降低，事实上，一个状态还可能转移到 SG 值比它大的后继状态，如果一个玩家这样做，那么另一个玩家只需将该状态降回原来即可（SG 值大的总是可以降低为 SG 值小的），因此升高 SG 值是无用的。

## SG 定理

设 $A+B$ 表示两个相互独立的游戏组合而成的游戏，$\oplus$ 表示异或。基于上面的讨论，我们发现当 $SG(A)\oplus SG(B)=0$ 时，组合游戏为必败状态，即 $SG(A+B)=0$；当 $SG(A)\oplus SG(B)\ne 0$ 时，组合游戏为必胜状态，即 $SG(A+B)\ne 0$。我们猜测 $SG(A+B)=SG(A)\oplus SG(B)$，证明如下。

设 $C,D$ 分别表示 $A,B$ 的任意后继状态，即 $A\to C,B\to D$。则 $A+B$ 的局面要么移动 $A$，要么移动 $B$，即 $(A+B)\to\{C+B\}\cup \{A+D\}$。

由定义可得，$SG(A+B)=\operatorname{mex}\big\{\{SG(C+B)\}\cup \{SG(A+D)\}\big\}$。

考虑数学归纳法，对两个有向图进行拓扑排序，拓扑序最大的点为出度为 $0$ 的点。当 $A,B$ 都为拓扑序最大的点时，$SG(A+B)=SG(A)\oplus SG(B)$ 显然成立。假设拓扑序比 $A,B$ 大的点都满足定理，则有 $SG(C+B)=SG(C)\oplus SG(B),SG(A+D)=SG(A)\oplus SG(D)$。

于是有 $SG(A+B)=\operatorname{mex}\big\{\{SG(C)\oplus SG(B)\}\cup \{SG(A)\oplus SG(D)\}\big\}$。下面将证明 $SG(A)\oplus SG(B)$ 不属于右边两个集合的任何一个，但比它小的非负整数都属于右边两个集合的某一个。

因为 $A\to C,B\to D$，所以 $SG(A)\ne SG(C),SG(B)\ne SG(D)$，则 $SG(A)\oplus SG(B)$ 不等于 $SG(C)\oplus SG(B)$ 和 $SG(A)\oplus SG(D)$。

设 $e$ 是比 $SG(A)\oplus SG(B)$ 小的任意非负整数，$k=SG(A)\oplus SG(B)\oplus e$。令 $SG(A),SG(B),e$ 三者分别与 $k$ 异或，则至少有一者异或后会减小（因为 $k$ 的二进制最高位的 $1$ 一定来自这三者之一），但 $e\oplus k=SG(A)\oplus SG(B)>e$，因此这一者不会是 $e$。不妨设这一者为 $SG(A)$，即 $SG(A)\oplus k=SG(B)\oplus e<SG(A)$。由 $SG$ 函数的定义可知，$A$ 一定存在一个后继状态 $C^{'}$，满足 $SG(C^{'})=SG(B)\oplus e$，则此时有 $SG(C^{'})\oplus SG(B)=e$，因此 $e$ 属于右边两个集合的某一个。原命题得证。

显然这个定理可以推广至多个游戏，设 $X$ 为 $n$ 个独立的有向图游戏的组合游戏，它们的起点分别为 $s_1,s_2,\dots,s_n$，则有 $SG(X)=SG(s_1)\oplus SG(s_2)\oplus \dots \oplus SG(s_n)$。

## 经典模型

博弈论中有很多经典的游戏，很多博弈都可以转化为这些游戏。

### 巴什博弈

有一堆石子，数量为 $n$，两个人轮流拿石子，每个人每次至少拿 $1$ 个，至多拿 $m$ 个，最终拿完石子的人获胜。

设 $SG(k)$ 表示 $k$ 个石子的 SG 值，显然有 $SG(0)=0$，

当 $k\le m$ 时，$k$ 可以转移到 $0$ 到 $k-1$ 任意一个局面，因此 $SG(k)=k,(k\le m)$；

当 $k=m+1$ 时，可以转移到 $1$ 到 $m$ 任意一个局面，则 $SG(m+1)=0$；

当 $k=m+2$ 时，可以转移到 $2$ 到 $m+1$ 任意一个局面，则 $SG(m+2)=1\dots\dots$

以此类推，可以得到 $SG(n)=n\bmod (m+1)$。当且仅当 $(m+1) \mid n$ 时先手必败，否则先手必胜。

### Nim 游戏

有 $n$ 堆石子，第 $i$ 堆有 $a_i$ 个石子，两个玩家轮流取走任意一堆的任意个石子，但不能不取，拿完石子的人获胜。

这 $n$ 堆石子相互独立，因此可以看作 $n$ 个独立的游戏。对于一堆有 $k$ 个石子的游戏，显然有 $SG(k)=k$。因此 Nim 游戏的 SG 值为 $a_i\oplus a_2\oplus \dots \oplus a_n$，称其为 Nim 和。

考虑先手必胜时要如何操作，当 Nim 和为 $k(k>0)$ 时，设 $k$ 的二进制最高位的 $1$ 在第 $d$ 位，则一定存在 $a_i$ 满足其二进制第 $d$ 位为 $1$，于是 $a_i\oplus k <a_i$，因此只需将 $a_i$ 取到 $a_i\oplus k$ 即可。

### 阶梯 Nim

有 $n$ 堆石子，第 $i(i>0)$ 堆有 $a_i$ 个石子，两人轮流操作，每人每次可以任选一个 $k(k>0)$，将第 $k$ 堆石子中的任意个石子移动到第 $k-1$ 堆，第 $0$ 堆的石子无法移动，最后无法操作的人输。

结论：先手必败当且仅当奇数堆中的石子数异或和为 $0$。

证明：当奇数堆异或和为 $0$ 时，无论怎么移动都会使奇数堆的异或和大于 $0$。如果先手移动偶数堆的石子到奇数堆，后手可以将移动到奇数堆的石子再往下移动到偶数堆，异或和就变回了 $0$；如果先手移动奇数堆，可以看作两人在奇数堆上玩 Nim 游戏，所以后手可以通过移动奇数堆的石子使异或和变回 $0$。

综上，奇数堆异或和为 $0$ 的局面都由先手拿到，异或和不为 $0$ 的局面都由后手拿到。无法移动的局面为所有石子都在第 $0$ 堆，此时奇数堆的异或和为 $0$，因此该局面由先手拿到，故先手必败。

### k-Nim

有 $n$ 堆石子，第 $i$ 堆有 $a_i$ 个石子，每次可以从不超过 $k$ 堆中各取走任意个石子，不能操作的人输。

结论：设 $s_i$ 表示 $a_1,a_2\dots ,a_n$ 二进制下第 $i$ 位 $1$ 的个数。则先手必败当且仅当对所有的 $s_i$，都有 $s_i\equiv 0 \pmod{(k+1)}$。
 
证明：

设 $s_i^{'}=s_i\bmod (k+1)$。

$a_i$ 全为 $0$ 时为必败状态，此时满足条件。

若所有的 $s_i^{'}$ 为 $0$，由于最多选择 $k$ 堆，因此 $s_i$ 的变化量的绝对值不会超过 $k$，并且至少有一个 $s_i$ 产生变化，所以该操作结束后一定会使得某些 $s_i^{'}$ 不为 $0$。

若 $s_i^{'}$ 不全为 $0$，下面将证明一定存在一种合法的方案使得所有的 $s_i^{'}$ 变回 $0$。

考虑数学归纳法，假设当前在二进制下的第 $i$ 位，满足对所有的 $j>i$，$s_j^{'}$ 都已经变为了 $0$，并且已经改变了 $m(m\le k)$ 堆石子。

显然对于已经改变的 $m$ 堆石子，我们任意改变其二进制下第 $i$ 位的值都是合法的。设这 $m$ 堆石子的第 $i$ 位上有 $a$ 个 $1$ 和 $b$ 个 $0$。

下面分情况讨论，

1. $a\ge s_i^{'}$，只需要从这 $a$ 个中选择 $s_i^{'}$ 个将其变为 $0$ 即可。

2. $b\ge (k+1)-s_i^{'}$，只需要从这 $b$ 个中选择 $(k+1)-s_i^{'}$ 个将其变为 $1$ 即可。

3. 上述两种情况都不满足，即 $a<s_i^{'}$ 且 $b<(k+1)-s_i^{'}$。那么先将这 $a$ 个 $1$ 变为 $0$，然后再从这 $m$ 堆之外选 $s_i^{'}-a$ 堆第 $i$ 位为 $1$ 的，并将其变为 $0$ 即可，此时选择的总堆数变为了 $m+s_i^{'}-a=b+s_i^{'}$，由于 $b<(k+1)-s_i^{'}$，所以 $b+s_i^{'}<k+1$，这种取法是合法的。

综上，一定存在一种合法的方案使得所有的 $s_i^{'}$ 变回 $0$。

故 $s_i^{'}$ 全为 $0$ 为必败状态，$s_i^{'}$ 不全为 $0$ 为必胜状态，结论得证。

### 威佐夫博弈

有两堆石子，数量任意，可以不同。游戏开始由两个人轮流取石子。游戏规定，每次有两种不同的取法：一是可以在任意的一堆中取走任意多的石子；二是可以在两堆中同时取走相同数量的石子。最后把石子全部取完者为胜者。

结论：设石子数量为 $a,b(a<b)$，当且仅当 $a=\lfloor \frac{\sqrt{5}+1}{2} \rfloor\times (b-a)$ 时先手必败。

证明太抽象了，[自己看题解吧](https://www.luogu.com.cn/problem/solution/P2252)。

## 例题

博弈论常用解题方法有：转化为经典模型；手算几个寻找结论；打表 SG 找规律；通过一些方法维护 SG 函数；将原游戏转化为一个新的游戏。

### 【模板】Nim 游戏 

[题目链接](https://www.luogu.com.cn/problem/P2197)

模板题。

{% spoiler "代码" %}

```cpp
#include <bits/stdc++.h>
typedef long long ll;
using namespace std;
const int N=1e3+7;

int main()
{
    int T;
    cin>>T;
    while(T--) {
        int n,ans=0;
        scanf("%d",&n);
        for(int i=1;i<=n;++i) {
            int x;
            scanf("%d",&x);
            ans^=x;
        }
        if(ans!=0)  puts("Yes");
        else  puts("No");
    }
    return 0;
}
```

{% endspoiler %}

### 【模板】威佐夫博弈 

[题目链接](https://www.luogu.com.cn/problem/P2252)

模板题。

{% spoiler "代码" %}

```cpp
#include <bits/stdc++.h>
typedef long long ll;
using namespace std;
const int N=5e5+7;

int main()
{
    double k=(sqrt(5.0)+1)/2.0;
    int a,b;
    cin>>a>>b;
    if(a>b)  swap(a,b);
    if(a==int((b-a)*k))  puts("0");
    else  puts("1");

    return 0;
}
```

{% endspoiler %}

### CF388C Fox and Card Game 

[题目链接](https://www.luogu.com.cn/problem/CF388C)

桌子上有 $n(n\le 100)$ 堆牌，每堆有 $s_i(s_i\le 100)$ 张牌。每张牌上都有一个正整数。C 可以从任何非空牌堆的顶部取出一张牌，J 可以从任何非空牌堆的底部取出一张牌。C 先取，当所有的牌堆都变空时游戏结束。他们都想最大化他所拿牌的分数（即每张牌上正整数的和）。问他们所拿牌的分数分别是多少？

{% spoiler "题解" %}

先考虑所有牌堆都为偶数的情况，结论为对于每堆牌，先手会取走顶部的一半，后手会取走底部的一半，将这个局面记为稳定局面，设此时先手有 $a$ 分，后手有 $b$ 分。

证明：设先后手最终分别获得 $x,y$ 分。无论一方怎么拿牌，另一方都可以与对方对照着拿牌来迫使最终达到稳定局面，因此他们获得的分数一定不会低于稳定局面，即 $x\ge a,y\ge b$，又因为两人拿到的分数之和不变，即 $x+y=a+b$，因此 $x=a,y=b$。

接下来考虑奇数牌堆，对于某一个奇数牌堆，某一方取走一张牌后，这个牌堆就变为了偶数堆，两人各分一半，因此，谁先取了奇数牌堆，谁就多拿走了中间那张牌。所以先手会取中间牌最大的奇数牌堆，后手会取第二大的，先手再取第三大的，就这样一直取下去，直到没有奇数牌堆。剩下就一人一半就行了。

用大根堆将奇数牌堆按中间牌从大到小排序，依次取出即可。时间复杂度 $O(n\log n)$。

{% endspoiler %}

{% spoiler "代码" %}

```cpp
#include <bits/stdc++.h>
typedef long long ll;
using namespace std;
const int N=1e2+7;
int a[N][N],b[N][N];
priority_queue<int> q;
int main()
{
    int n,ans1=0,ans2=0;
    cin>>n;
    for(int i=1;i<=n;++i) {
        int m;
        scanf("%d",&m);
        for(int j=1;j<=m;++j) {
            scanf("%d",&a[i][j]);
            b[i][j]=b[i][j-1]+a[i][j];
        }
        ans1+=b[i][m/2];ans2+=b[i][m]-b[i][(m+1)/2];
        if(m&1)  q.push(a[i][(m+1)/2]);
    }
    bool flag=0;
    while(!q.empty()) {
        if(!flag)  ans1+=q.top();
        else  ans2+=q.top();
        flag^=1;q.pop();
    }
    cout<<ans1<<" "<<ans2;

    return 0;
}
```

{% endspoiler %}

### CF794E Choosing Carrot

[题目链接](https://www.luogu.com.cn/problem/CF794E)

有一排 $n(n\le 3\times 10^5)$ 个数，甲乙轮流取，每次可以选择两端中的一端取走一个数，直到剩下一个数，甲希望剩下的数最大，乙希望最小。同时，**在游戏开始前**，甲可以提前操作 $k(0\le k\le n-1)$ 次（操作规则和上面一样），**游戏开始后**甲是先手。问对于每一个 $k$，最后剩下来的数是几。

{% spoiler "题解" %}

先考虑 $k=0$ 的情况，如果 $n$ 是偶数，设 $mid=\frac{n}{2}$，与上题类似，无论先手怎么取，后手都可以与先手相对着取，则可以保证最后留下来的数不大于 $\max\{ a_{mid},a_{mid+1}\}$。而对于先手，如果 $a_{mid}>a_{mid+1}$，先手就先取右端，之后无论后手怎么取，先手都和对方相对着取，这样就留下了 $a_{mid}$，当 $a_{mid}<a_{mid+1}$ 时也同理，因此先手可以保证最后留下来的数不小于 $\max\{ a_{mid},a_{mid+1}\}$。综上，最后留下来的数一定为 $\max\{ a_{mid},a_{mid+1}\}$。

如果 $n$ 是奇数，设 $mid=\frac{n+1}{2}$先手任取一端后就变为了偶数的情况，但此时是后手拿到偶数的情况，因此后手会留下中间两个中较小的一个，而先手希望最后留下来的最大，因此答案为 $\max\left\{ \min\{ a_{mid-1},a_{mid}\},\min\{ a_{mid},a_{mid+1}\} \right\}$。

接下来考虑 $k>0$ 的情况，当留下的区间确定后该情况的答案也确定了，先手会选择所有情况中最大的答案。假设 $n$ 为偶数，设 $mid=\frac{n}{2}$，当 $k=0$ 时答案为 $\max\{ a_{mid},a_{mid+1}\}$；

当 $k=2$ 时答案为 $\max\left\{ \max\{ a_{mid-1},a_{mid}\},\max\{ a_{mid},a_{mid+1}\},\max\{ a_{mid+1},a_{mid+2}\} \right\}$；

当 $k=4$ 时答案为 $\max\left\{ \max\{ a_{mid-2},a_{mid-1}\},\dots,\max\{ a_{mid+2},a_{mid+3}\} \right\}$。

可以发现当 $n$ 为偶数时，$k+2$ 相当于在 $k$ 的答案的基础上左右扩展一个。同理可以推得 $n$ 为奇数时也符合这个结论。注意要特判 $k=n-1$ 的情况。

时间复杂度为 $O(n)$。

{% endspoiler %}

{% spoiler "代码" %}

```cpp
#include <bits/stdc++.h>
typedef long long ll;
using namespace std;
const int N=3e5+7,inf=1e9+7;
int a[N],b[N];
int main()
{
    int n;
    cin>>n;
    for(int i=1;i<=n;++i)  scanf("%d",&a[i]);
    if(n==1) {cout<<a[1];return 0;}
    for(int i=2;i<n;++i)  b[i]=max(min(a[i-1],a[i]),min(a[i],a[i+1]));
    int max0=-inf,max1=-inf,h=n/2,t;
    if(n&1)  t=h+2,max1=b[h+1],printf("%d ",max1),max0=a[h+1];
    else  t=h+1,max0=max(a[h],a[t]),printf("%d ",max0);
    for(int i=n-1;i>=2;--i) {
        if(i&1)  max1=max(max1,max(b[h],b[t])),--h,++t,printf("%d ",max1); 
        else  max0=max(max0,max(a[h],a[t])),printf("%d ",max0);
    }
    printf("%d ",max0);
    return 0;
}
```

{% endspoiler %}

### [AGC002E] Candy Piles 

[题目链接](https://www.luogu.com.cn/problem/AT_agc002_e)

桌上有 $n(n\le 10^5)$ 堆糖果，第 $i$ 堆糖果有 $a_i$ 个糖。两人在玩游戏，轮流进行，每次可以将当前最大的那堆糖果全部吃完，或者将每堆糖果吃掉一个。吃完的人输，假设两人足够聪明，问谁有必胜策略？

{% spoiler "题解" %}

将这 $n$ 堆糖果按数量从多到少依次排序，每堆糖果排成一排，设左上角为 $(1,1)$，如图，

![](https://a154051-img-1321592229.cos.ap-beijing.myqcloud.com/img/23-12-20-1.png)

如果将当前最大的那堆糖果全部吃完（操作一），就会变成 $(2,1)$，

![](https://a154051-img-1321592229.cos.ap-beijing.myqcloud.com/img/23-12-20-2.png)

如果将每堆糖果吃掉一个（操作二），就会变成 $(1,2)$，

![](https://a154051-img-1321592229.cos.ap-beijing.myqcloud.com/img/23-12-20-3.png)

于是发现，设当前在 $(i,j)$，则执行一次操作一会变成 $(i+1,j)$，相当于向右走一步；执行一次操作二会变成 $(i,j+1)$，相当于向下走一步。

显然如果一个点的右边和下边都没有点，那该点为必败点。一个点为必胜点当且仅当它的右边和下边至少有一个必败点，一个点为必败点当且仅当它的右边和下边都为必胜点。

则有结论：$(i,j)$ 和 $(i+1,j+1)$ 的胜败状态相同。

证明：设 $0$ 表示必败，$1$ 表示必胜。若 $(i,j)$ 为 $1$，则 $(i+1,j)$ 和 $(i,j+1)$ 中至少有一个为 $0$，因此 $(i+1,j+1)$ 一定为 $1$。

若 $(i,j)$ 为 $0$，则 $(i+1,j)$ 和 $(i,j+1)$ 都为 $1$。考虑反证法，假设 $(i+1,j+1)$ 为 $1$，则 $(i+2,j)$ 和 $(i,j+2)$ 必须为 $0$，进而有 $(i+2,j+1)$ 和 $(i+1,j+2)$ 都为 $1$，由此可推出 $(i+1,j+1)$ 为 $0$，与假设矛盾，因此 $(i+1,j+1)$ 为 $0$。

综上，$(i,j)$ 和 $(i+1,j+1)$ 的胜败状态相同。

因此只需从 $(1,1)$ 一直往右下方走，直到右下方没有点，此时只能一直向右走或者一直向下走，则只需判断右边和下边剩余点的奇偶即可。

时间复杂度 $O(n)$。

{% endspoiler %}

{% spoiler "代码" %}

```cpp
#include <bits/stdc++.h>
typedef long long ll;
using namespace std;
const int N=1e5+7;
int a[N];
bool cmp(int x,int y) {return x>y;}
int main()
{
    int n;
    cin>>n;
    for(int i=1;i<=n;++i)  scanf("%d",&a[i]);
    sort(a+1,a+1+n,cmp);
    int mx=0;
    for(int i=1;i<=n;++i) 
        if(a[i]>=i)  mx=i;
    if((a[mx]-mx)&1)  puts("First");
    else {
        int cnt=0;
        for(int i=mx+1;i<=n;++i) {
            if(a[i]<mx)  break;
            ++cnt;
        }
        if(cnt&1)  puts("First");
        else  puts("Second");
    }
    
    return 0;
}
```

{% endspoiler %}

### SP11414 COT3 - Combat on a tree

[题目链接](https://www.luogu.com.cn/problem/CF1110G)

两人在一棵 $n(n\le 10^5)$ 个节点的树上玩游戏，根节点为 $1$，每个节点最初都是黑色或白色，他们轮流执行以下操作：从当前树中选择一个白色节点 $v$，将路径 $(1,v)$ 上的所有白色节点都变为黑色。无法操作的玩家失败，问先手是否能够必胜，并求出第一步的方案。

{% spoiler "题解" %}

考虑维护每个点的 SG 值，设 $SG(i)$ 表示以 $i$ 为根的子树的 SG 值，$SG(i)(k)$ 表示在 $i$ 子树内选择点 $k$ 进行操作后的 SG 值，$son_i$ 表示 $i$ 的儿子数量，$i_j$ 表示 $i$ 的第 $j$ 个儿子。

如果 $i$ 是黑点，可以发现 $i$ 的儿子间都是独立的，因此 $SG(i)=SG(i_1)\oplus SG(i_2)\oplus \dots \oplus SG(i_{son_i})$，
但只求 SG 值是不够的，因为题目要求第一步选哪些点可以必胜，也就是从 $i$ 的所有后继状态中找出 SG 值为 $0$ 的状态，因此我们还需要维护 $i$ 的所有后继状态，考虑到每个点都至多有 $n$ 个后继状态，因此这是可行的。

设 $s_i=SG(i_1)\oplus SG(i_2)\oplus \dots \oplus SG(i_{son_i})$，若任选 $i$ 子树中的一点 $k$，设 $k$ 在 $i_j$ 子树内，则当前局面的 SG 值为 $SG(i)(k)=SG(i_1)\oplus \dots \oplus SG(i_{j-1})\oplus SG(i_j)(k)\oplus SG(i_{j+1})\oplus \dots \oplus SG(i_{son_i})$，即 $SG(i)(k)=s_i\oplus SG(i_j)\oplus SG(i_j)(k)$，
这是 $i$ 的一个后继状态。我们发现 $SG(i_j)(k)$ 是 $i_j$ 的一个后继状态，由于 $i_j$ 能选的点，$i$ 也可以选，因此 $i_j$ 的任意一个后继状态异或上 $s_i\oplus SG(i_j)$ 就变成了 $i$ 的后继状态。

这样就得到了一种解法，但一个个异或复杂度太高，我们需要一个能够区间异或的数据结构，自然想到了 01Trie，对每个节点都建一个 01Trie 来保存该子树的所有后继状态的 SG 值，要求 $i$ 的 01Trie，只需将 $i$ 的每一个儿子 $i_j$ 的 01Trie 整体异或上 $s_i\oplus SG(i_j)$，然后将所有儿子的 01Trie 合并就得到了 $i$ 的 01Trie，合并时可以用启发式合并或者线段树合并。需要注意由于要求第一步的方案，因此还要维护每个 SG 值是由哪些点产生的，合并时用启发式合并。

上面是 $i$ 为黑点的情况，而当 $i$ 为白点时，任选一点后，$i$ 就变成了黑点，就和上面一样了，唯一不同的是多了一个选择点 $i$ 的状态。因此白点就是在黑点的 01Trie 上再插入一个 $s_i$。

如果使用线段树合并，时间复杂度为 $O(n\log n)$；使用启发式合并为 $O(n\log^2 n)$。

{% endspoiler %}

{% spoiler "代码" %}

```cpp
//线段树合并
#include <bits/stdc++.h>
typedef long long ll;
using namespace std;
const int N=1e5+7,LEN=17;
struct node {
    int ch[2],tag,id,siz;
}tr[N*18];
int c[N],tot=0,rt[N],sg[N],tot_id=0,ANS[N];
vector<int> nx[N],num[N];
void push(int x,int k)
{
    if(k==0)  return ;
    if((tr[x].tag)&(1<<(k-1)))  swap(tr[x].ch[0],tr[x].ch[1]);
    if(tr[x].ch[0])  tr[tr[x].ch[0]].tag^=tr[x].tag;
    if(tr[x].ch[1])  tr[tr[x].ch[1]].tag^=tr[x].tag;
    tr[x].tag=0;
}
void merge(int &now,int re,int k)
{
    if(!now||!re)  {now=now+re;return ;}
    if(k==0) {
        tr[now].tag=0;
        int x=tr[now].id,y=tr[re].id;
        if(!x||!y)  tr[now].id+=tr[re].id;
        else {
            if(num[x].size()<num[y].size())  swap(x,y),swap(tr[now].id,tr[re].id);
            for(int i=0;i<num[y].size();++i)  num[x].push_back(num[y][i]);
        }
        return ;
    }
    if(tr[now].tag!=0)  push(now,k);
    if(tr[re].tag!=0)  push(re,k);
    merge(tr[now].ch[0],tr[re].ch[0],k-1);
    merge(tr[now].ch[1],tr[re].ch[1],k-1);
    tr[now].siz=tr[tr[now].ch[0]].siz+tr[tr[now].ch[1]].siz+1;
}
void ins(int &now,int v,int k,int ID)
{
    now=++tot;
    if(k==0) {
        tr[now].id=++tot_id;
        num[tot_id].push_back(ID);
        tr[now].siz=1;
        return ;
    }
    int tmp=(v>>(k-1))&1;
    ins(tr[now].ch[tmp],v,k-1,ID);
    tr[now].siz=tr[tr[now].ch[tmp]].siz+1;
}
int mex(int now,int k)
{
    if(!now)  return 0;
    int ans=0;
    while(1) {
        if(!now)  return ans;
        if(tr[now].tag!=0)  push(now,k);
        if(tr[now].siz==(1<<(k+1))-1)  return ans+(1<<k);
        if(tr[tr[now].ch[0]].siz==(1<<k)-1)  ans+=(1<<(k-1)),now=tr[now].ch[1];
        else  now=tr[now].ch[0];
        --k;
    }
    return ans;
}
void dfs(int x,int fa)
{
    int tmp=0;
    for(int i=0;i<nx[x].size();++i) {
        int y=nx[x][i];
        if(y==fa)  continue;
        dfs(y,x);
        tmp^=sg[y];
    }
    if(!c[x])  ins(rt[x],tmp,LEN,x);
    for(int i=0;i<nx[x].size();++i) {
        int y=nx[x][i];
        if(y==fa)  continue;
        if(rt[y])  tr[rt[y]].tag^=tmp^sg[y];
        merge(rt[x],rt[y],LEN);
    }
    sg[x]=mex(rt[x],LEN);
}
int main()
{
    int n;
    cin>>n;
    for(int i=1;i<=n;++i)  scanf("%d",&c[i]);
    for(int i=1;i<n;++i) {
        int x,y;
        scanf("%d%d",&x,&y);
        nx[x].push_back(y);
        nx[y].push_back(x);
    }
    dfs(1,0);
    if(sg[1]==0)  puts("-1");
    else {
        int now=rt[1];
        for(int i=LEN;i>0;--i) {
            if(tr[now].tag!=0)  push(now,i);
            now=tr[now].ch[0];
        }
        int cnt=num[tr[now].id].size();
        for(int i=0;i<cnt;++i)  ANS[i]=num[tr[now].id][i];
        sort(ANS,ANS+cnt);
        for(int i=0;i<cnt;++i)  printf("%d ",ANS[i]);
    }
    return 0;
}
```

{% endspoiler %}

### CF1451F Nullify The Matrix

[题目链接](https://www.luogu.com.cn/problem/CF1451F)

有一个 $n\times m$ 的棋盘，每个格子有一个非负整数的权值。

两个人轮流操作，每次操作选择两个权值非 $0$ 的格子，左上的点为起点，右下的为终点，每次移动只能向右走一格或者向下走一格，首先给起点的权值减一个正整数，然后从起点移动到终点，路径上每个点的权值随意加减（也可以不改变），但是都不能改成负数。

不能操作（全变成 $0$）的人就输了，问谁必胜。

{% spoiler "题解" %}

设当前在 $(i,j)$，则每走一步都会使 $i+j$ 加上 $1$。将 $i+j$ 相同的分为一组，记为第 $i+j$ 组，也就是将每一斜行分为一组，设 $s_{i}$ 表示 $i$ 组内所有格子权值的异或和，则有结论：当且仅当所有的 $s_i$ 都为 $0$ 时先手必败。

证明：当所有的 $s_i$ 都为 $0$ 时，由于起点的权值只能减小，因此起点所在的组的异或和在操作后一定不为 $0$。

当存在 $s_i$ 不为 $0$ 时，则找到最小的 $i$ 使得 $s_i\ne 0$，将第 $i$ 组看作一个 Nim 游戏，则一定存在一种合法方案能够降低某个格子的值并使得 $s_i$ 变为 $0$，然后从该点出发，以 $(n,m)$ 为结尾，就可以从 $s_{i+1}$ 遍历到 $s_{n+m}$。对于每一个 $s_j (i+1\le j \le n+m)$，若 $s_j=0$ 则保持不变；若 $s_j\ne 0$，则修改其中任意一个格子的权值使得 $s_j=0$ 即可。

时间复杂度 $O(nm)$。

{% endspoiler %}

{% spoiler "代码" %}

```cpp
#include <bits/stdc++.h>
typedef long long ll;
using namespace std;
const int N=1e2+7;
int a[N][N];
int main()
{
    int T;
    cin>>T;
    while(T--) {
        int n,m;
        scanf("%d%d",&n,&m);
        for(int i=1;i<=n;++i)
            for(int j=1;j<=m;++j)
                scanf("%d",&a[i][j]);
        bool flag=0;
        for(int i=2;i<=n+m;++i) {
            int sum=0;
            for(int j=1,k=i-1;j<=n&&k>=1;++j,--k)
                if(k<=m)  sum^=a[j][k];
            if(sum) {flag=1;break;}
        }
        if(flag)  puts("Ashish");
        else  puts("Jeel");
    }
    return 0;
}
```

{% endspoiler %}

### CF1149E Election Promises

[题目链接](https://www.luogu.com.cn/problem/CF1149E)

给定一张有向无环图，每个点有一个非负整数点权。两人轮流操作，每次操作为选择一个权值为不为 $0$ 的点，并将它的值**降低**为一个非负整数，对于它的出点（即走一步能到达的点），可以将其值任意修改为一个非负整数（可以保持不变）。最终无法操作的人输，问先手能否必胜，如果必胜请输出第一步的方案。

{% spoiler "题解" %}

和上题类似，我们可以将每个点分组，使得它们满足：任意操作都会使必败状态变为必胜状态，必胜状态一定能够通过合法的操作变为必败状态。

我们令每个点和它的所有出点都不在同一组，设每个组的权值 $s_i$ 为组内所有点权值的异或和，当且仅当所有组都为 $0$ 时为必败状态，这样就满足了任意操作都会使其变为必胜状态。接下来考虑如何分组才能使得必胜状态能够转移到必败状态。

在上题中选择一个点后可以修改其后的所有组，类似地，我们希望对于每一组，选择其中任意一点后可以修改该组前面的所有组。也就是该组前面的每一组都有该点的出点，又因为当前组没有出点，自然想到了 $\operatorname{mex}$ 操作，一个点所在的组为：对其所有出点所在的组的编号进行 $\operatorname{mex}$ 操作所得到的数。于是对原图进行拓扑排序，按拓扑序的逆序进行分组即可。

这样对于必胜状态，只需找到编号最大的权值不为 $0$ 的组，该组的点的出点所在的组一定包含了前面所有组，就和上一题一样修改就行了，输出方案也比较简单。

时间复杂度 $O(n)$。

{% endspoiler %}

{% spoiler "代码" %}

```cpp
#include <bits/stdc++.h>
typedef long long ll;
using namespace std;
const int N=2e5+7;
vector<int> nx[N],last[N],num[N];
ll a[N],sum[N];
int deg[N],col[N];
deque<int> q;
bool vis[N];
int main()
{
    int n,m;
    cin>>n>>m;
    for(int i=1;i<=n;++i)  scanf("%lld",&a[i]);
    for(int i=1;i<=m;++i) {
        int x,y;
        scanf("%d%d",&x,&y);
        nx[x].push_back(y);
        last[y].push_back(x);
        ++deg[x];
    }
    for(int i=1;i<=n;++i)  
        if(!deg[i]) {
            q.push_back(i);
            col[i]=1;
            sum[1]^=a[i];
            num[1].push_back(i);
        }
    int mx=1;
    while(!q.empty()) {
        int x=q.front();
        q.pop_front();
        for(int i=0;i<last[x].size();++i) {
            int y=last[x][i];
            --deg[y];
            if(!deg[y]) {
                q.push_back(y);
                for(int j=0;j<nx[y].size();++j)
                    vis[col[nx[y][j]]]=1;
                int mex=1;
                while(vis[mex])  ++mex;
                col[y]=mex;sum[mex]^=a[y];
                mx=max(mx,mex);
                num[mex].push_back(y);
                for(int j=0;j<nx[y].size();++j)
                    vis[col[nx[y][j]]]=0;
            }
        }
    }
    int flag=0;
    for(int i=mx;i;--i) {
        if(!sum[i])  continue;
        int cnt=1;
        while(sum[i]>>cnt)  ++cnt;
        for(int j=0;j<num[i].size();++j) {
            int x=num[i][j];
            if(a[x]&(1<<(cnt-1))) {a[x]^=sum[i];flag=x;break;}
        }
        for(int j=0;j<nx[flag].size();++j) {
            int x=nx[flag][j];
            if(sum[col[x]])  a[x]^=sum[col[x]],sum[col[x]]=0;
        }
    }
    if(!flag)  puts("LOSE");
    else  {
        puts("WIN");
        for(int i=1;i<=n;++i)  printf("%lld ",a[i]);
    }
    return 0;
}
```

{% endspoiler %}

### P3235 [HNOI2014] 江南乐

[题目链接](https://www.luogu.com.cn/problem/P3235)

[写过题解了](https://a154051.gitee.io/2023/12/06/sol-P3235-jiang-nan-le/)


## 参考资料

[公平组合游戏 - OI Wiki](https://oi-wiki.org/math/game-theory/impartial-game/)

[10170 Sprague-Grundy定理是怎么想出来的 - 王赟 Maigo](https://zhuanlan.zhihu.com/p/20611132?utm_id=0)




