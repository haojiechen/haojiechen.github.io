---
layout: post
title: 'hdu 6070-Dirt Ratio(二分+线段树)'
subtitle: '算法篇'
date: 2017-9-8
categories: 算法
tags: 算法 线段树 二分
---
# HDU-6070 Dirt Ratio(二分+线段树)
__传送门__:   [点这里~](http://acm.hdu.edu.cn/showproblem.php?pid=6070) 
## 题意
给你一个数列，让你求一个连续的区间，使得这个区间内不同数的个数除以区间长度的值最小。
## Solution
### __二分__

>因为所求的这个值在 (0,1] 范围之内，所以我们可以采用二分的方法来找到这个值。只需要每次判断当前的值是否满足条件即可。  

__条件如下:__

定义函数diff(l,r)为区间[l,r]中不同的数的个数，mid为当前二分得到的值，那么应该满足:  

<center>diff(l,r) / (r-l+1)<=mid</center>  
显然，这样的公式我们并没有一个好的方法来进行判断，所以我们可以将其变形如下:  

<center>diff(l,r) + l*mid <= mid*(r+1)</center>   
这样，我们枚举每一个右端点，然后借助线段树来解决。  

### __线段树__

采用线段树，线段树中结点的值存储的为不等式的左半边，即为f(l,r)=diff(l,r) + l*mid。枚举右端点，我们令线段树叶子结点的含义为该结点位置l，以及当前枚举的右端点的位置r，的f(l,r)的值，父结点的值为子节点的最小值。我们可以存储下来已经枚举的值最右边的位置pre，然后每次只需要更新[pre+1,r]这个区间。具体结合代码理解。
```java
#include<bits/stdc++.h>
using namespace std;
const int MAXN = 60000+100;
const int M = MAXN*4;
int tag[M],ar[MAXN],mr[MAXN];
double v[M],mid;
void build(int l,int r,int rt)
{
    tag[rt]=0;
    if(l==r){v[rt]=l*mid;return;}
    int m = l+(r-l)/2;
    build(l,m,rt<<1);
    build(m+1,r,rt<<1|1);
    v[rt]=min(v[rt<<1],v[rt<<1|1]);
}
void pd(int rt,int p)
{
    tag[rt]+=p;v[rt]+=p;
}
void change(int rt,int l,int r,int a,int b)
{
    if(l>=a&&r<=b) //懒惰标记
    {
        tag[rt]++;v[rt]++;
        return;
    }
    if(tag[rt]) pd(rt<<1,tag[rt]),pd(rt<<1|1,tag[rt]),tag[rt]=0;
    int m = l+(r-l)/2;
    if(m>=a) change(rt<<1,l,m,a,b);
    if(b>m) change(rt<<1|1,m+1,r,a,b);
    v[rt]=min(v[rt<<1],v[rt<<1|1]);
}
bool ok(int rt,int l,int r,int a)
{
    if(r<=a)  
    {
        if(v[rt]<=mid*(a+1))
            return true;
        return false;
    }
    if(tag[rt]) pd(rt<<1,tag[rt]),pd(rt<<1|1,tag[rt]),tag[rt]=0;
    int m = l+(r-l)/2;
    bool f = ok(rt<<1,l,m,a);
    if(!f&&a>m) f=ok(rt<<1|1,m+1,r,a);
    return f;
}
main()
{
    ios::sync_with_stdio(false);
    int T;cin>>T;
    while(T--)
    {
        int n;cin>>n;
        for(int i=1;i<=n;i++)
            cin>>ar[i];
        double l=0,r=1;
        for(int o=0;o<=40;o++)
        {
            mid = (l+r)/2;
            build(1,n,1);
            bool flag=0;
            for(int i=0;i<=n;i++) mr[i]=0; //每个值的最右边的位置
            for(int i=1;i<=n;i++)
            {
                change(1,1,n,mr[ar[i]]+1,i); //更新
                mr[ar[i]]=i;
                if(ok(1,1,n,i)){flag=1;break;}
            }
            if(flag) r=mid;
            else l=mid;
        }
        cout<<setprecision(10)<<fixed<<(l+r)/2<<endl;
    }
}

```

