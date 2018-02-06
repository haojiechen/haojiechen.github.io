---
layout: post
title: 'hdu 6041-I Curse Myself(第k小生成树)'
subtitle: '算法'
date: 2017-9-4
cover: /assets/img/I Curse Myself-Solution.jpg
categories: 算法
tags: ACM
---
# HDU-6041 I Curse Myself (第k小生成树)
__传送门__:   [点这里~](http://acm.hdu.edu.cn/showproblem.php?pid=6041) 
## 题意：
给你一个无向带权图，保证每条边仅会出现在一个环中，让你求这个图的前k小生成树的权和。
## Solution
题上说每条边仅会出现在一个环中，很容易想到 __找出图中的所有环，把每个环去掉一条边，所剩的部分必然是一棵生成树。__ 这样问题就转换为：m个集合（每个环的所有边权是一个集合），从每个集合挑出来一个元素进行组合（生成树的权和=图中所有边的权和-该组合的值），求前k大的组合的和。  

__这样就把问题分解为：__
- 怎样找出所有的环
- 如何进行集合的合并  

### 怎样找环
找环我们可以通过并查集来实现，对于每条新加入的边， __如果两个端点有同一个“父亲”，那么这条边必然存在于一个环中。__ 我门先单独把这些边保存起来（不加入图中），然后根据两个端点进行dfs找到所在的环即可。

### 集合的合并
两个集合的元素两两相加组成一个新的集合，然后继续拿这个新的集合与下一个集合进行合并，合并完后进行排序，取前k大。这样的思路没什么错误，但是不管从时间上还是空间上都让人无法接受。由于我们只需要知道前k大就够了，我们可以借助堆来实现，每次合并只取前k大即可，详见代码。

## Code
Talk is cheap , show me the code.
```c++
#include<bits/stdc++.h>
#define int long long
using namespace std;
const int MAXN = 1000+100;
const int M = 3000;
const int MO=(1LL<<32);
int n,m,k,ca,cb;
struct node{
    int v,w,nex;
}e[M*2];
int EN,head[M*2],fa[MAXN],A[100010],B[100010];
struct edge{
    int u,v,w;
    edge(int u,int v,int w):u(u),v(v),w(w){}
    edge(){}
};
vector<edge> ve;
void init()
{
    memset(head,-1,sizeof head);
    EN = 0;
    ve.clear();
    ca=0;cb=0;
    for(int i=0;i<=n;i++)fa[i]=i;
}
void add(int u,int v,int w)
{
    e[EN].v=v,e[EN].w=w,e[EN].nex=head[u];
    head[u]=EN++;
}
int fand(int x)
{
    return fa[x]==x?x:fa[x]=fand(fa[x]);
}
bool dfs(int u,int f,int en)
{
    if(u==en)return true;
    for(int i=head[u];~i;i=e[i].nex)
    {
        if(i==f) continue;
        if(dfs(e[i].v,i^1,en))
        {
            B[cb++]=e[i].w;
            return true;
        }
    }
    return false;
}
bool cmp(int a,int b)
{
    return a>b;
}
void merg()
{
    if(ca > cb) swap(ca,cb),swap(A,B);
    int siz = min(k,ca*cb);
    priority_queue<pair<int,int> >q;
    for(int i=0;i<ca;i++) q.push(make_pair(A[i]+B[0],0));
    ca=siz;
    for(int i=0;i<siz;i++)
    {
        pair<int,int>p = q.top();q.pop();
        A[i] = p.first;
        if(p.second+1<cb) q.push(make_pair(p.first-B[p.second]+B[p.second+1],p.second+1));
    }
}
main()
{
    ios::sync_with_stdio(false);
    int tt=0;
    while(cin>>n>>m)
    {
        init();
        int sum=0;
        int u,v,w;
        while(m--)
        {
            cin>>u>>v>>w;
            if(fand(u)!=fand(v))
            {
                add(u,v,w);
                add(v,u,w);
                fa[fand(u)]=fand(v);
            }else
            {
                ve.push_back(edge(u,v,w));
            }
            sum+=w;
        }cin>>k;
        for(auto E : ve)
        {
            cb=0;
            B[cb++]=E.w;
            dfs(E.u,-1,E.v);
            sort(B,B+cb,cmp);
            if(ca==0)
            {
                for(int i=0;i<cb;i++)
                    A[i]=B[i];
                ca=cb;
            }else
                merg();
        }
        int ans=0;
        for(int i=0;i<ca;i++)
        {
            ans+=(sum-A[i])*(i+1);
            ans%=MO;
        }
        if(ans==0) ans=sum;
        ans%=MO;
        cout<<"Case #"<<++tt<<": "<<ans<<endl;
    }
}	
```


