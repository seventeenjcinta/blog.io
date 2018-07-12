---
layout: post
title: Benelux Algorithm Programming Contest 2014 Final - E - Excellent Engineers
date: 2018-07-12
categories: blog
tags: [ACM,数据结构,线段树,思维]
use_math: true
description: 挡不住风霜
header-img: "img/Excellent Engineers.jpg"
---




<center><h1><font face="verdana" color="red"> Excellent Engineers </font></h1></center>
<center><font size="3" face="arial"> Time Limit: 2000 </font></center>	 
<center><font size="3" face="arial"> Memory Limit: 262144K </font></center>	 	



### Description

You are working for an agency that selects the best software engineers from Belgium, the Netherlands and Luxembourg for employment at various international companies. Given the very large number of excellent software engineers these countries have to offer, the agency has charged you to develop a tool that quickly selects the best candidates available from the agency's files.

Before a software engineer is included in the agency's files, he has to undergo extensive testing. Based on these tests, all software engineers are ranked on three essential skills: communication skills, programming skills, and algorithmic knowledge. The software engineer with rank one in the category algorithmic knowledge is the best algorithmic expert in the files, with rank two the second best, etcetera.

For potential customers, your tool needs to process the agency's files and produce a shortlist of the potentially most interesting candidates. A software engineer is a potential candidate that is to be put on this shortlist if there is no other software engineer in the files that scores better on all three skills. That is, an engineer is to be put on the shortlist if there is no other software engineer that has better communication skills, better programming skills, and more algorithmic knowledge.



### Input

On the first line one positive number: the number of test cases, at most 100100. After that per test case:

- one line with a single integer $n (1 \le n \le 10^5)$, the number of software engineers in the agency's files.
- $n$ lines, each with three space-separated integers $r_1r , r_2,r_3(1 \le r_1, r_2, r_3 \le n)$: the rank of each software engineer from the files with respect to his communication skills, programming skills and algorithmic knowledge, respectively.

For each skill $s$ and each rank $x$ between $1$ and $n$, there is exactly one engineer with $r_s = x$



### Output

Per test case:

- one line with a single integer: the number of candidates on the shortlist.



### Sample Input

3<br>
3<br>
2 3 2<br>
3 2 3<br>
1 1 1<br>
3<br>
1 2 3<br>
2 3 1<br>
3 1 2<br>
10<br>
1 7 10<br>
3 9 7<br>
2 2 9<br>
5 10 8<br>
4 3 5<br>
7 5 2<br>
6 1 3<br>
9 6 6<br>
8 4 4<br>
10 8 1<br>



### Sample Output

1<br>
3<br>
7<br>

***

考完后久违的写题<br>
给你 $n$ 个满足偏序关系的三元组<br>
如果其中一个元素小于另一个元素<br>
就会把那个元素给吃了<br>
问你最后会剩下多少个元素<br>

三位偏序<br>
很显然可以通过对 $x$ 排序降维<br>
然后遍历 $x$<br>
对于到第 $i$ 个元素的时候<br>
显然 $x_i$ 肯定会比之前的更大<br>
所以他不被吃的充要条件是<br>
对于之前出现过的 $y < y_i$<br>
所对应的 $z > z_i$<br>
所以我们对于 $<y,z>$ 建立线段树<br>
每次对于一个新元素<br>
查询区间 $[1, y_i - 1]$ 的最小值<br>
如果最小值小于 $z_i$<br>
就跳过<br>
否则就把 $<y_i, z_i>$ 插入线段树中<br>
即 $y_i = z_i$





<pre><code>
#include <bits/stdc++.h>
using namespace std;

const int N = 100010;
const int INF = 0x3f3f3f3f;

struct Node
{
    int x;
    int y;
    int z;
};

struct Tree
{
    int l;
    int r;
    int minn;
};

Tree tree[N << 2];
Node node[N];
int n;

bool cmp(Node a, Node b)
{
    return a.x < b.x;
}

void Up(int x)
{
    tree[x].minn = min(tree[x << 1].minn, tree[x << 1 | 1].minn);
}

void Build(int x, int l, int r)
{
    tree[x].l = l;
    tree[x].r = r;
    tree[x].minn = INF;
    if(l == r){
        return;
    }
    else{
        int mid;

        mid = l + ((r - l) >> 1);
        Build(x << 1, l, mid);
        Build(x << 1 | 1, mid + 1, r);
        Up(x);
    }
}

void Update(int x, int l, int r, int v)
{
    int ll;
    int rr;

    ll = tree[x].l;
    rr = tree[x].r;
    if(l > r){
        return ;
    }
    if(ll == l && rr == r){
        tree[x].minn = v;
    }
    else{
        int mid;
    
        mid = ll + ((rr - ll) >> 1);
        Update(x << 1, l, min(mid, r), v);
        Update(x << 1 | 1, max(mid + 1, l), r, v);
        Up(x);
    }
}

int Quary(int x, int l, int r)
{
    int ll;
    int rr;

    ll = tree[x].l;
    rr = tree[x].r;
    if(l > r){
        return INF;
    }
    if(ll == l && rr == r){
        return tree[x].minn;
    }
    else{
        int mid;
        int t;
    
        t = INF;
        mid = ll + ((rr - ll) >> 1);
        t = min(t, Quary(x << 1, l, min(r, mid)));
        t = min(t, Quary(x << 1 | 1, max(mid + 1, l), r));
        Up(x);
    
        return t;
    }
}

int main(int argc, char const *argv[])
{
    int ncase;
    int ans;

    scanf("%d", &ncase);
    while(ncase --){
        ans = 0;
        scanf("%d", &n);
        for(int i = 0; i < n; i ++){
            scanf("%d%d%d", &node[i].x, &node[i].y, &node[i].z);
        }
        sort(node, node + n, cmp);
        Build(1, 1, n);
        for(int i = 0; i < n; i ++){
            if(Quary(1, 1, node[i].y - 1) < node[i].z){
                continue;
            }
            else{
                ans ++;
                Update(1, node[i].y, node[i].y, node[i].z);
            }
        }
        printf("%d\n", ans);
    }
    
    return 0;
}
