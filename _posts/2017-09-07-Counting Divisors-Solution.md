---
layout: post
title: 'hdu 6069-Counting Divisors'
subtitle: '算法篇'
date: 2017-9-7
categories: 算法
tags: ACM
---
# HDU-6069 Counting Divisors
__传送门__:   [点这里~](http://acm.hdu.edu.cn/showproblem.php?pid=6069) 
## 题意
定义函数`d(n)`为`n`的因子个数，让你求区间`【l,r】`中所有`d(i) (l<=i<=r)`函数的和。
## Solution 
同样这是一个计数问题，由于`l`和`r`都很大，所以无法暴力求解。我们只能考虑每个素数对于它的倍数的贡献。对于一个数`n`来说，由唯一分解定理可知，`n = p1^k1 * p2^k2 * ... * pm^km`，其中`pi`为素数，`ki`为次方。从组合数的角度来看，对于`n`的每个素因子`pi`，我们可以任取`0-ki`个，那么`n`的因子的个数就应该是`(k1+1) * (k2+1) * ... * (km + 1)`。这样，我们就可以枚举` √r` 以内的所有素数，分别算出区间`【l,r】`中每个数含有该素数的个数，对应相乘即可。具体看代码。
## Code
Talk is cheap , show me the code.
```c++
#include<bits/stdc++.h>
#define int long long
using namespace std;
const int MAXN = 1e6+100;
const int mod = 998244353;
int p[MAXN],pri[MAXN];
void init()
{
    int sz=0;
    for(int i=2;i<MAXN;i++)
    {
        if(!p[i]) pri[sz++]=i;
        for(int j=0;j<sz;j++)
        {
            int t=i*pri[j];
            if(t>=MAXN)break;
            p[t]=1;
            if(i%pri[j]==0) break;
        }
    }
}
int a[MAXN],b[MAXN];
main()
{
    ios::sync_with_stdio(false);
    init();
    int l,r,k,T;
    cin>>T;
    while(T--)
    {
        cin>>l>>r>>k;
        for(int i=0;i<=r-l;i++)
        {
            a[i]=1;  //存放i+l的因子个数
            b[i]=i+l; //用于判断 i+l 这个数是否还含有大于√r的素数
        }
        for(int i=0;pri[i]*pri[i]<=r;i++)
        {
            int t=l/pri[i]*pri[i];
            while(t<l)t+=pri[i];
            while(t<=r)
            {
                int cnt=0;
                while(b[t-l]%pri[i]==0)
                {
                    cnt++;
                    b[t-l]/=pri[i];
                }
                a[t-l]*=(cnt*k+1);a[t-l]%=mod;
                t+=pri[i];
            }
        }
        int ans=0;
        for(int i=0;i<=r-l;i++)
        {
            if(b[i]!=1) ans+=a[i]*(k+1)%mod;
            else ans+=a[i];
            ans%=mod;
        }
        cout<<ans<<endl;
    }
}
```
