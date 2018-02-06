---
layout: post
title: 'hdu 6052-To my boyfrind'
subtitle: '算法篇'
date: 2017-9-7
categories: 算法
tags: ACM
---
# HDU-6052 To my boyfrind
__传送门__:   [点这里~](http://acm.hdu.edu.cn/showproblem.php?pid=6052) 
## 题意
给你一个`n*m`大小的矩阵，矩阵中每个数字`i(0<=i<=n*m)`代表一种颜色，让你求随机选择一个子矩阵，子矩阵中不同颜色个数的期望，即 __用每个子矩阵的不同颜色的个数之和除以子矩阵的总个数。__  
  
- __求子矩阵总个数__  
只需要两重`for`循环来枚举矩阵的左上点，然后右下点可以在该点的右下方任意位置，即对于`(i,j)`点，它向右下方向的矩阵个数为`(n-i)*(m-j)`。其实不难发现，对于`n*m`大小的矩阵，其子矩阵的个数为`n*(n+1)*m*(m+1)/4`。
- __求每个子矩阵不同颜色的个数__  
对于此类问题，大多情况下都不是暴力去算每个矩阵中不同颜色的个数，而是转换思路，求每个颜色的贡献。为了避免重复计算或者漏算，我们规定每个子矩阵中相同颜色的贡献为最左上的点的贡献，其他相同颜色的点不做贡献。这样对于每种颜色，找出它的所能贡献的矩阵个数即可。具体如下：  
<img src = "/assets/img/To my boyfriend1.png"/>  
假设图中的点是我们正在统计的颜色，灰色点是已经统计过的，黑色点是正在统计的，白色点是待统计的，那么对于黑色点来说，包含它的矩阵不能包含灰色点，因为灰色点已经被统计过，可以包含白色点，也可以不包含白色点，所以它的范围为:<img src = "/assets/img/To my boyfriend2.png">  
我们可以从黑色点当前行开始向上，统计每一行，每一行在黑色点左边的点为左上点，黑色点右下方的点为右下点，可以构成的矩阵的个数。详见代码。
## Code
Talk is cheap , show me the code.
```c++
#include<bits/stdc++.h>
#define int long long
using namespace std;
const int MAXN = 100+10;
const int mod = 1e9+7;
int mu,zi;
int n,m;
bool vis[MAXN*MAXN];
vector<int>colo;
vector<pair<int,int> >col[MAXN*MAXN];
void calc(int co)
{
    sort(col[co].begin(),col[co].end());
    int len = col[co].size();
    for(int i=0;i<len;i++)
    {
        int l=0,r=m+1;
        int x = col[co][i].first;
        int y = col[co][i].second;
        for(int j=i-1;j>=0;j--)
        {
            zi+=(y-l)*(r-y)*(n+1-col[co][i].first)*(x-col[co][j].first);
            x=col[co][j].first;
            if(col[co][j].second<y) l=max(l,col[co][j].second);
            else if(col[co][j].second>y) r=min(r,col[co][j].second);
            else {l=r=y;break;}
        }
        zi+=(y-l)*(r-y)*(n+1-col[co][i].first)*x;
        //cout<<col[co][i].first<<" "<<col[co][i].second<<" "<<zi<<endl;
    }
    //cout<<x<<" "<<y<<" "<<zi<<endl;
}
main()
{
    ios::sync_with_stdio(false);
    int t;cin>>t;
    while(t--)
    {
        mu=zi=0;cin>>n>>m;
        for(int i=0;i<=n*m;i++){vis[i]=0;col[i].clear();}
        int a;colo.clear();
        for(int i=1;i<=n;i++)
            for(int j=1;j<=m;j++)
            {
                cin>>a;
                col[a].push_back(make_pair(i,j));
                if(!vis[a]){vis[a]=1;colo.push_back(a);}
            }
        int len = colo.size();
        for(int i=0;i<len;i++) calc(colo[i]);
        mu = n*(n+1)/2*m*(m+1)/2;
        //cout<<zi<<" "<<mu<<endl;
        cout<<setprecision(9)<<fixed<<(double)zi/mu<<endl;
    }
    return 0;
}
```
