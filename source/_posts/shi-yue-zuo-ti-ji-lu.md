---
author: a154051
avatar: 'https://a154051-img-1321592229.cos.ap-beijing.myqcloud.com/img/头像new.jpg'
title: 十月做题记录
tags:
  - 做题记录
mathjax: true
date: 2023-10-24 16:10:00
categories: 题解
description: 记录摆烂的十月
keywords: 十月做题记录
photos: https://a154051-img-1321592229.cos.ap-beijing.myqcloud.com/img/23-10-24.jpg
---

加星号表示看了题解。

摆烂的十月份。

## P1709 [USACO5.5] 隐藏口令 Hidden Password ＊

 [题目链接](https://www.luogu.com.cn/problem/P1709)
 
最小表示法模板题，设当前比较以 $i$ 和 $j$ 开头的字符串，则对两字符串一位一位向后比较，即每次比较 $i+k$ 和 $j+k$，直到遇到不同的字符，假设 $i$ 开头的字符串更小，则令 $j$ 直接跳到 $j+k+1$，因为对于以 $j$ 到 $j+k$ 开头的字符串，都有对应的以 $i$ 到 $i+k$ 开头的字符串必它小。时间复杂度 $O( n)$。


{% spoiler "代码" %}

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N=5e6+7;
char a[N];
int main()
{
    int n;
    cin>>n;
    for(int i=0;i<n;++i) {
        char ch=getchar();
        while(ch<'a'||ch>'z')  ch=getchar();
        a[i]=ch;
    }
    int i=0,j=1,k=0;
    while(k<n&&i<n&&j<n) {
        if(a[(i+k)%n]==a[(j+k)%n])  ++k;
        else {
            a[(i+k)%n]>a[(j+k)%n]?i=i+k+1:j=j+k+1;
            if(i==j)  ++i;
            k=0;
        }
    }
    cout<<min(i,j);
    return 0;
}
```

{% endspoiler %}

## P6310 「Wdsr-1」仓库建设 

[题目链接](https://www.luogu.com.cn/problem/P6310)

写过题解了，[【题解】P6310 [Wdsr-1]仓库建设](https://a154051.gitee.io/2023/09/25/solution-P6310-cang-ku-jian-she/)



## 武汉大学2023年新生程序设计竞赛（同步赛）

###  A. 教科书般的亵渎

[题目链接](https://ac.nowcoder.com/acm/contest/66651/A)

将 $a$ 从大到小排序，若第一项为 $0$ 或 $1$ 且后面每一项与前一项的差小于等于 $1$ 则为 $YES$，否则为 $NO$。

{% spoiler "代码" %}

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N=1e6;
int a[N];
int main()
{
    int n;
    cin>>n;
    for(int i=1;i<=n;++i)  scanf("%d",&a[i]);
    sort(a+1,a+1+n);
    int last=0;
    for(int i=1;i<=n;++i) {
        if(a[i]==last||a[i]==last+1) {last=a[i];continue;}
        cout<<"NO";
        return 0;
    }
    cout<<"YES";
    return 0;
}
```

{% endspoiler %}

### C. 覆叶之交

[题目链接](https://ac.nowcoder.com/acm/contest/66651/C)

给定三个矩形，求它们的面积并。

用容斥写，难点在代码实现。

{% spoiler "代码" %}

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
struct squ {
    bool flag;
    ll x1,x2,y1,y2;
}a[20];
int tot=3,tmp[10];

void sol(squ A,squ B)
{
    ++tot;
    if(A.flag||B.flag) {a[tot].flag=1;return ;}
    if(A.x2<=B.x1||B.x2<=A.x1||A.y2<=B.y1||B.y2<=A.y1)  {a[tot].flag=1;return ;}
    tmp[1]=A.x1;tmp[2]=A.x2;tmp[3]=B.x1;tmp[4]=B.x2;
    sort(tmp+1,tmp+5);
    a[tot].x2=tmp[3];a[tot].x1=tmp[2];

    tmp[1]=A.y1;tmp[2]=A.y2;tmp[3]=B.y1;tmp[4]=B.y2;
    sort(tmp+1,tmp+5);
    a[tot].y1=tmp[2];a[tot].y2=tmp[3];
}
int main()
{
    ll ans=0;
    for(int i=1;i<=3;++i) {
        cin>>a[i].x1>>a[i].y1>>a[i].x2>>a[i].y2;
        ans+=abs((a[i].x1-a[i].x2)*(a[i].y1-a[i].y2));
    }
    for(int i=1;i<=3;++i)
        for(int j=i+1;j<=3;++j) {
            sol(a[i],a[j]);
            if(a[tot].flag==0)  ans-=abs((a[tot].x1-a[tot].x2)*(a[tot].y1-a[tot].y2));
        }
    sol(a[4],a[5]);sol(a[6],a[7]);
    if(a[tot].flag==0)  ans+=abs((a[tot].x1-a[tot].x2)*(a[tot].y1-a[tot].y2));
    cout<<ans;
    return 0;
}
```

{% endspoiler %}

### E. 不是n皇后问题

[题目链接](https://ac.nowcoder.com/acm/contest/66651/E)

看着挺麻烦，实际上就是把 $1$ 到 $n^2$ 按顺序填进格子就行了。

{% spoiler "代码" %}

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N=1e6;
int main()
{
    int n,cnt=0;
    cin>>n;
    for(int i=1;i<=n;++i) {
        for(int j=1;j<=n;++j)  printf("%d ",++cnt);
        printf("\n");
    }
        

    return 0;
}
```

{% endspoiler %}

### J. 放棋子

[题目链接](https://ac.nowcoder.com/acm/contest/66651/J)

行和列可以分开计算，对于同一行，要使分数最大，则每次落子需要相连的旗子尽可能多，因此可以从左至右依次落子，这样就可以得到这一行的最大分数。可以证明，如果从第一行到最后一行操作也可以得到列的最大分数。时间复杂度 $O(n\times m)$。

{% spoiler "代码" %}

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const int N=1e5+7;
vector<int> a[N];
int main()
{
    int n,m;
    ll ans=0;
    cin>>n>>m;
    for(int i=1;i<=n;++i) {
        a[i].push_back(0);
        ll now=0;
        for(int j=1;j<=m;++j) {
            char ch=getchar();
            while(ch!='#'&&ch!='.')  ch=getchar();
            a[i].push_back(0);
            if(ch=='#') {
                a[i][j]=1;
                ++now;
                ans+=now*now;
            } 
            else  now=0;
        }
    }
    for(int i=1;i<=m;++i) {
        ll now=0;
        for(int j=1;j<=n;++j) {
            if(a[j][i])  ++now,ans+=now*now;
            else  now=0;
        }
    }
    cout<<ans;
    return 0;
}
```

{% endspoiler %}

### K. 矩形分割

[题目链接](https://ac.nowcoder.com/acm/contest/66651/K)

显然我们想要分割出来的正方形边长尽可能大，所以以矩形的短边为正方形边长进行分割直到无法分割，如果还剩下来一个小矩形，就再对这个矩形进行上述操作，直到分割完全。


{% spoiler "代码" %}

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
int sol(int n,int m)
{
    if(n==0||m==0)  return 0;
    if(n==m)  return n;
    if(n<m)  swap(n,m);
    return m*(n/m)+sol(m,n%m);

}
int main()
{
    int n,m;
    cin>>n>>m;
    cout<<sol(n,m);
    return 0;
}
```

{% endspoiler %}

### L. 小镜的数学题

[题目链接](https://ac.nowcoder.com/acm/contest/66651/L)

从 $x$ 向后暴力找，最坏也不会超过 $2x$，数据比较水就过了。

{% spoiler "代码" %}

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
int main()
{
    ll n;
    cin>>n;
    for(ll i=n+1;;++i) {
        if((n&i)==0) {
            cout<<i;
            return 0;
        }
        n=(n&i);
    }
    return 0;
}
```

{% endspoiler %}

## P2017 [USACO09DEC] Dizzy Cows G 

[题目链接](https://www.luogu.com.cn/problem/P2017)

给定一个图，有无向边和有向边，给每条无向边指定一个方向，并且不出现环。

考虑拓扑排序判定有向无环图的过程，每次删去出度为 $0$ 的点，若最终能删完则为有向无环图，可以发现，每条边都是由拓扑序大的指向拓扑序小的。因此在本题中先无视无向边对原图进行拓扑排序，再让无向边由拓扑序大的点指向拓扑序小的点，就得到了有向无环图。

{% spoiler "代码" %}

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N=1e5+7;
vector<int> last[N];
int deg[N],dep[N],cnt=0;
deque<int> q;
int main()
{
    int n,m1,m2;
    cin>>n>>m1>>m2;
    for(int i=1;i<=m1;++i) {
        int x,y;
        scanf("%d%d",&x,&y);
        ++deg[x];
        last[y].push_back(x);
    }
    for(int i=1;i<=n;++i)
        if(!deg[i])  q.push_back(i);
    while(q.size()) {
        int x=q.front();
        q.pop_front();
        dep[x]=++cnt;
        for(int i=0;i<last[x].size();++i) 
            if((--deg[last[x][i]])==0)  q.push_back(last[x][i]);
        
    }
    while(m2--) {
        int x,y;
        scanf("%d%d",&x,&y);
        if(dep[x]<dep[y])  swap(x,y);
        printf("%d %d\n",x,y);
    }
    return 0;
}
```

{% endspoiler %}

## 2023 年华中科技大学程序设计竞赛新生赛（线上同步赛）

几乎每道题都会犯sb错误，我是超级罚时王。

### P9769 [HUSTFC 2023] 简单的加法乘法计算题 

[题目链接](https://www.luogu.com.cn/problem/P9769)

设 $f_i$ 表示从 $0$ 到 $i$ 的最小操作次数，考虑最后一次操作，要么是加上 $A$  中的一个数，要么是乘上 $B$ 中的一个数，所以 $f_i=\min\{ \min\limits_{j=1}^n f_{i-A_j},\min\limits_{k=1}^m f_{i/B_k}\}$，加法用堆来维护，乘法一个个枚举，时间复杂度 $O(ym\log n)$，如果用单调队列可以降到 $O(ym)$。


{% spoiler "代码" %}

```cpp
#include <bits/stdc++.h>
#define mp(x,y) make_pair(x,y)
typedef long long ll;
using namespace std;
const int N=5e6+7;
int f[N],b[20];
priority_queue<pair<int,int> > q;
bool vis[N];
int main()
{
    int x,n,m;
    cin>>x>>n>>m;
    for(int i=1;i<=m;++i)  scanf("%d",&b[i]);
    vis[0]=1;q.push(mp(0,0));
    for(int i=1;i<=x;++i) {
        int tmp=q.top().second;
        while(!vis[tmp])  q.pop(),tmp=q.top().second;
        f[i]=f[tmp]+1;
        for(int j=1;j<=m;++j)
            if(i%b[j]==0)  f[i]=min(f[i],f[i/b[j]]+1);
        q.push(mp(-f[i],i));
        vis[i]=1;
        if(i-n-1>=0)  vis[i-n-1]=0;
    }
    cout<<f[x];
    return 0;
}
```

{% endspoiler %}

### P9771  [HUSTFC 2023] 排列排序问题 

[题目链接](https://www.luogu.com.cn/problem/P9771)

显然切割出来的序列是单调的，并且相邻的两个数相差 $1$，按这个方法切割使每个切出来的序列尽可能大就行了。时间复杂度 $O(n)$。



{% spoiler "代码" %}

```cpp
#include <bits/stdc++.h>
typedef long long ll;
using namespace std;
const int N=1e6+7;
int a[N];
int main()
{
    int n;
    cin>>n;
    for(int i=1;i<=n;++i)  scanf("%d",&a[i]);
    int flag=-1,last=a[1],ans=0;
    for(int i=2;i<=n;++i) {
        if(flag==-1) {
            if(a[i]==last+1)  flag=1,last=a[i];
            else  if(a[i]==last-1)  flag=0,last=a[i];
            else  last=a[i],++ans;
            continue;
        }
        if(a[i]==last+1&&flag==1) {last=a[i];continue;}
        if(a[i]==last-1&&flag==0) {last=a[i];continue;}
        last=a[i];++ans;
        flag=-1;
    }
    cout<<ans;
    return 0;
}
```

{% endspoiler %}

### P9774[HUSTFC 2023] 新取模运算 

[题目链接](https://www.luogu.com.cn/problem/P9774)

显然新定义的运算符满足分配律，于是对于 $n$ 到 $1$ 这 $n$ 个数，可以分为是 $p$ 的倍数和不是 $p$ 的倍数分别计算，最后再相乘。

对于不是 $p$ 的倍数的数，它们对 $p$ 进行新定义运算等价于直接对 $p$ 取模，这一部分可以通过预处理 $1$ 到 $p$ 的阶乘来求解。

对于是 $p$ 的倍数的数，例如 $p,2p,3p...kp$，它们对 $p$ 进行新定义运算需要先除以 $p$，变为 $1,2,3...k$，这就成了原问题的子问题，于是可以递归求解，边界是 $ k<p$。时间复杂度 $O(\log_p n \times \log n)$。


{% spoiler "代码" %}

```cpp
#include <bits/stdc++.h>
typedef long long ll;
using namespace std;
const int N=1e6+7;
int p;
ll jc[N];
ll qpow(ll x,ll k)
{;
    ll ans=1,tmp=x;
    while(k) {
        if(k&1)  ans=ans*tmp%p;
        tmp=tmp*tmp%p;
        k>>=1;
    }
    return ans;
}
ll sol(ll n)
{
    ll tmp=n/p;
    ll ans=qpow(jc[p-1],tmp)*jc[n%p]%p;
    if(tmp)  ans=ans*sol(tmp)%p;
    return ans;
}
int main()
{
    int T;
    cin>>T>>p;
    jc[0]=1;
    for(int i=1;i<=p;++i)  jc[i]=jc[i-1]*i%p;
    while(T--) {
        ll n;
        scanf("%lld",&n);
        printf("%lld\n",sol(n));
    }
    return 0;
}
```

{% endspoiler %}

### P9775 [HUSTFC 2023] 广义线段树 

[题目链接](https://www.luogu.com.cn/problem/P9775)

给定一棵 $2n-1$ 个节点的树，$1$ 是根节点，$n$ 到 $2n-1$ 是叶子节点，给定叶子节点的点权，其他节点的点权是以它为子树的所有叶子节点的乘积。令每个叶子节点乘上一个数，求最后所有节点的和。

对叶子节点乘上一个数，则从叶子节点到根节点路径上所有节点都要乘上这个数。所以原问题转化为对树上的一条链乘上一个数，可以用树链剖分，时间复杂度 $O(n\log^2 n)$。比赛时没细想，但实际上可以先对所有叶子节点做修改，再在 dfs 返回时向上传就可以了，时间复杂度 $O(n)$。


{% spoiler "代码" %}

```cpp
#include <bits/stdc++.h>
typedef long long ll;
using namespace std;
const int N=2e6+7,mod=998244353;
struct tree {
    ll tag,val;
}tr[N<<1];
ll a[N],b[N];
int nx[N][2],fa[N],dep[N],siz[N],top[N],pos[N],son[N],tot=0,rev[N];
void push(int now,ll k)
{
    tr[now].val=tr[now].val*k%mod;
    tr[now].tag=tr[now].tag*k%mod;
    return ;
}
void build(int now,int l,int r)
{
    tr[now].tag=1;
    if(l==r) {tr[now].val=a[rev[l]];return ;}
    int mid=(l+r)>>1;
    build(now<<1,l,mid);
    build(now<<1|1,mid+1,r);
    tr[now].val=(tr[now<<1].val+tr[now<<1|1].val)%mod;
}
void mul(int now,int l,int r,int x,int y,ll k)
{
    if(l>=x&&r<=y) {
        tr[now].val=tr[now].val*k%mod;
        tr[now].tag=tr[now].tag*k%mod;
        return ;
    }
    if(tr[now].tag!=1) {
        push(now<<1,tr[now].tag);
        push(now<<1|1,tr[now].tag);
        tr[now].tag=1;
    }
    
    int mid=(l+r)>>1;
    if(x<=mid)  mul(now<<1,l,mid,x,y,k);
    if(y>mid)  mul(now<<1|1,mid+1,r,x,y,k);
    tr[now].val=(tr[now<<1].val+tr[now<<1|1].val)%mod;
}

void dfs1(int x,int FA)
{
    fa[x]=FA;dep[x]=dep[FA]+1;siz[x]=1;
    if(nx[x][0]) {
        dfs1(nx[x][0],x);dfs1(nx[x][1],x);
        siz[x]+=siz[nx[x][0]]+siz[nx[x][1]];
        son[x]=siz[nx[x][0]]>siz[nx[x][1]]?nx[x][0]:nx[x][1];
        a[x]=a[nx[x][0]]*a[nx[x][1]]%mod;
    }
}
void dfs2(int x,int tp)
{
    top[x]=tp;pos[x]=++tot;rev[tot]=x;
    if(!son[x])  return ;
    dfs2(son[x],tp);
    int tmp=nx[x][0];
    if(tmp==son[x])  tmp=nx[x][1];
    dfs2(tmp,tmp);
}
void chg(int x,ll k)
{
    if(k==1)  return ;
    while(x) {
        mul(1,1,tot,pos[top[x]],pos[x],k);
        x=fa[top[x]];
    }
}
int main()
{
    int n;
    cin>>n;
    for(int i=n;i<=n*2-1;++i)  scanf("%lld",&a[i]);
    for(int i=1;i<=n;++i)  scanf("%lld",&b[i]);
    for(int i=1;i<n;++i)  scanf("%d%d",&nx[i][0],&nx[i][1]);
    dfs1(1,0);
    dfs2(1,1);
    build(1,1,tot);
    for(int i=1;i<=n;++i) {
        chg(i+n-1,b[i]);
        printf("%lld ",tr[1].val);
    }
    return 0;
}
```

{% endspoiler %}

### P9777 [HUSTFC 2023] Fujisaki 讨厌数学 

[题目链接](https://www.luogu.com.cn/problem/P9777)

已知 $x^1+x^{-1}=k$，求 $x^n+x^{-n}$。

可以发现 $(x^a+x^{-a})(x^b+x^{-b})=x^{a+b}+x^{-(a+b)}+x^{(a-b)}+x^{-(a-b)}$。设 $f_n=x^n+x^{-n}$，可以得到 $f_{(a+b)}=f_a \times f_b-f_{(a-b)}$。因此，若 $n$ 是偶数，有 $f_n=f_{n/2}^2-f_0$，若 $n$ 是奇数，有 $f_n=f_{n/2}\times f_{n/2+1}-f_1$。

设 $n$ 在第一层，向下拆分得到第二层，第二层要么有一个数要么有两个数，考虑两个数的情况，这两个数一定相差 $1$，将它们向下拆分，第三层仍然是两个数，并且也是相差 $1$，可以证明每一层都至多两个数。每次拆分都除以 $2$，因此有 $\log n$ 层。可以用记忆化搜索，但是 $n$ 太大了，要用 $map$ 存，时间复杂度 $O(\log^2 n)$。


{% spoiler "代码" %}

```cpp
#include <bits/stdc++.h>
typedef long long ll;
using namespace std;
map<ll,ll> mp;
const int N=1e6+7;
int mod;
ll k;
ll sol(ll n)
{
    if(n==1)  return k;
    if(mp.find(n)!=mp.end())  return mp[n];
    ll ans=1;
    if(n&1)  ans=sol(n/2)*sol(n/2+1)%mod-k;
    else  ans=sol(n/2),ans=ans*ans%mod-2;
    while(ans<0)  ans+=mod;
    mp[n]=ans;
    return ans;
}
int main()
{
    ll n;
    cin>>mod>>k>>n;
    if(n==0)  cout<<"2";
    else  printf("%lld",sol(n));

    return 0;
}
```

{% endspoiler %}

### P9779 [HUSTFC 2023] 不定项选择题 

[题目链接](https://www.luogu.com.cn/problem/P9779)

$n$ 道题一共有 $2^n$ 种情况，除去全都不选的情况，最坏情况是最后一次才试出来，即要试 $2^n-1$ 次。~~其实我是看样例猜出来的结论~~


{% spoiler "代码" %}

```cpp
#include <bits/stdc++.h>
typedef long long ll;
using namespace std;
const int N=2e5+7;

int main()
{
    int n,ans=1;
    cin>>n;
    for(int i=1;i<=n;++i)  ans*=2;
    cout<<ans-1;
    return 0;
}
```

{% endspoiler %}

### P9780 [HUSTFC 2023] Azur Lane 

[题目链接](https://www.luogu.com.cn/problem/P9780)

先考虑怎样使天数最小，一天内放置的喵箱等级是一个不上升序列，因此将序列划分为多个不上升序列，使每个序列尽可能大，就得到了天数最小的情况。

之后天数每增加一，就需要从已划分的序列中再划分出一个序列，同时使花费最少，显然天数靠前的喵箱越多花费就越多，因此我们希望喵箱尽可能放在后面，于是可以从第一天开始向后找到第一个序列长度大于 $1$ 的序列，将它划分出一个数，最后将答案加上天数增加导致前面喵箱增加的花费。时间复杂度 $O(n)$。


{% spoiler "代码" %}

```cpp
#include <bits/stdc++.h>
typedef long long ll;
using namespace std;
const int N=1e6+7;
int a[N];
ll b[N];
int main()
{
    int m,k,tot=1;
    ll ans=0;
    cin>>m>>k;
    for(int i=1;i<=m;++i)  scanf("%d",&a[i]);
    b[1]=1;
    for(int i=2;i<=m;++i)  {
        if(a[i]<=a[i-1])  ++b[tot];
        else  b[++tot]=1;
    }
    for(int i=1;i<=tot;++i)  ans+=b[i]*(tot-i+1);
    int now=1,totlast=0;
    for(int i=1;i<=m;++i) {
        if(i<tot)  {printf("-1 ");continue;}
        if(i==tot) {printf("%lld ",ans);continue;}
        while(b[now]==1)  ++totlast,++now;
        --b[now];++totlast;
        ans+=totlast;
        printf("%lld ",ans);
    }
    return 0;
}
```

{% endspoiler %}

### P9782 [HUSTFC 2023] A+B problem 

[题目链接](https://www.luogu.com.cn/problem/P9782)

签到题，$26$ 进制加法。


{% spoiler "代码" %}

```cpp
#include <bits/stdc++.h>
typedef long long ll;
using namespace std;
const int N=2e5+7;

int main()
{
    char ch1,tmp,ch2;
    scanf("%c%c%c",&ch1,&tmp,&ch2);

    int k=ch1-'A'+ch2-'A';
    if(k>=26) {
        k-=26;cout<<'B';
    }
    cout<<char(k+'A');
    return 0;
}
```

{% endspoiler %}

