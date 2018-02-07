---
layout: post
title: 常州大学新生寒假训练会试 - I
date: 2018-02-07
categories: blog
tags: [ACM,图论,思维]
description: 挡不住风霜
header-img: "img/changzhouI.jpg"
---




<center><h1><font face="verdana" color="red"> 合成反应r </font></h1></center>

<center><font size="3" face="arial"> Time Limit: 1000MS </font></center>	 
<center><font size="3" face="arial"> Memory Limit: 262144K </font></center>	 	



### Description

有机合成是指从较简单的化合物或单质经化学反应合成有机物的过程。
有时也包括从复杂原料降解为较简单化合物的过程。
由于有机化合物的各种特点，尤其是碳与碳之间以共价键相连，有机合成比较困难，常常要用加热、光照、加催化剂、加有机溶剂甚至加压等反应条件。
但是前人为有机合成提供了许多宝贵的经验。
现在已知有K总物质和N个前人已经总结出的合成反应方程式
小星想知道在现有M种物质的情况下 能否合成某些物质。

### Input

第一行输入四个整数 K,N,M,Q（K,N,M,Q<=1e5）
K表示一共K总物质
接下来N行 每行三个数字a b c（任意两个数可能相等）
表示a和b反应可以生成c 反应是可逆的
即可以通过c可以分解出a和b
接下来一行行然后输入m个数，表示m种原料（每一种原料都可以认为有无限多）
接下来Q个行Q个询问
对于每个询问
输出一个数字 x 判断是否可以通过一些反应得到 x

### Output

可以得到Yes否则No

### Sample Input

10 3 4 10
1 2 3
4 5 6
2 5 7
3 4 5 8
1
2
3
4
5
6
7
8
9
10

### Sample Output

Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
No
No

### Note

一共10总物质有第3，4，5，8 四种原料
查询每一种是否可以通过反应得到
首先通过3可以分解得到1 2
然后4 5合成6
2 5合成7
于是除了9 10都可以得到

***

题目链接：https://www.nowcoder.net/acm/contest/78/I
来源：牛客网
感谢：Vacant小星

蛮有趣的题目= =
细节就一个
然后被我写挫了
最后要到标程才发现
尴尬

对于每个关系 a + b = c
我们额外设立一个点 t
建立
a -> t 权值为 1
b -> t 权值为 1
t -> c 权值为 2
c -> a 权值为 2
c -> b 权值为 2

然后再设一个数组 val

以每个原料为起点
val 值设为 2 进行爆搜
其余每个点的权值等于到达该点的边的权值之和
val >= 2 为继续搜索的阈值


如果直接从 a b 连到 c 而不额外设一个点 t

5 2 2 1
1 2 3
4 5 3
1 4
3

会被这个数据 hack 掉


<pre><code>
#include &lt;bits/stdc++.h&gt;
#define pb push_back
using namespace std;

const int N = 200010;

struct Node
{
    vector<int> V;
    vector<int> W;
};

Node node[N];
int va[N];

void bfs(int x)
{
    queue<int> Q;

    va[x] = 2;
    Q.push(x);
    while(!Q.empty()){
        int now;
        int li;

        now = Q.front();
        Q.pop();
        if(va[now] < 2){
            continue;
        }
        li = node[now].V.size();
        for(int i = 0; i < li; i ++){
            int t;

            t = node[now].V[i];
            if(va[t] < 2){
                Q.push(t);
                va[t] += node[now].W[i];
            }
        }
    }
}

int main()
{
    int n, m, w, q;
    int cnt;

    while(scanf("%d%d%d%d", &n, &m, &w, &q) == 4){
        cnt = 1;
        memset(va, 0, sizeof(va));
        for(int i = 1; i <= n; i ++){
            node[i].V.clear();
            node[i].W.clear();
        }
        for(int i = 0; i < m; i ++){
            int a, b, c;

            scanf("%d%d%d", &a, &b, &c);
            node[c].V.pb(a);
            node[c].V.pb(b);
            node[c].W.pb(2);
            node[c].W.pb(2);
            node[a].V.pb(n + cnt);
            node[a].W.pb(1);
            node[b].V.pb(n + cnt);
            node[b].W.pb(1);
            node[n + cnt].V.pb(c);
            node[n + cnt].W.pb(2);
            cnt ++;
        }
        for(int i = 0; i < w; i ++){
            int x;

            scanf("%d", &x);
            if(va[x] < 2){
                bfs(x);
            }
        }
        while(q --){
            int x;

            scanf("%d", &x);
            if(va[x] >= 2){
                printf("Yes\n");
            }
            else{
                printf("No\n");
            }
        }
    }

    return 0;
}
/*
9 5 7 9
5 5 6
2 4 9
4 1 1
6 3 6
7 1 1
9 6 4 7 7 8 6 
1
2
3
4
5
6
7
8
9

*/
