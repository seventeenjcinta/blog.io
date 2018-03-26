---
layout: post
title: codeforces - 96D
date: 2018-03-26
categories: blog
tags: [ACM,图论,MST]
description: 挡不住风霜
header-img: "img/codeforces96D.jpg"
---




<center><h1><font face="verdana" color="red"> Volleyball </font></h1></center>

<center><font size="3" face="arial"> Time Limit: 2000MS </font></center>	 
<center><font size="3" face="arial"> Memory Limit: 256 megabytes </font></center>	 	



### Description

Petya loves volleyball very much. One day he was running late for a volleyball match. Petya hasn't bought his own car yet, that's why he had to take a taxi. The city has n junctions, some of which are connected by two-way roads. The length of each road is defined by some positive integer number of meters; the roads can have different lengths.

Initially each junction has exactly one taxi standing there. The taxi driver from the i-th junction agrees to drive Petya (perhaps through several intermediate junctions) to some other junction if the travel distance is not more than ti meters. Also, the cost of the ride doesn't depend on the distance and is equal to ci bourles. Taxis can't stop in the middle of a road. Each taxi can be used no more than once. Petya can catch taxi only in the junction, where it stands initially.

At the moment Petya is located on the junction x and the volleyball stadium is on the junction y. Determine the minimum amount of money Petya will need to drive to the stadium.

### Input

The first line contains two integers n and m (1 ≤ n ≤ 1000, 0 ≤ m ≤ 1000). They are the number of junctions and roads in the city correspondingly. The junctions are numbered from 1 to n, inclusive. The next line contains two integers x and y (1 ≤ x, y ≤ n). They are the numbers of the initial and final junctions correspondingly. Next m lines contain the roads' description. Each road is described by a group of three integers ui, vi, wi (1 ≤ ui, vi ≤ n, 1 ≤ wi ≤ 109) — they are the numbers of the junctions connected by the road and the length of the road, correspondingly. The next n lines contain n pairs of integers ti and ci (1 ≤ ti, ci ≤ 109), which describe the taxi driver that waits at the i-th junction — the maximum distance he can drive and the drive's cost. The road can't connect the junction with itself, but between a pair of junctions there can be more than one road. All consecutive numbers in each line are separated by exactly one space character.

### Output

If taxis can't drive Petya to the destination point, print "-1" (without the quotes). Otherwise, print the drive's minimum cost.

Please do not use the %lld specificator to read or write 64-bit integers in С++. It is preferred to use cin, cout streams or the %I64d specificator.

### Sample Input

4 4<br>
1 3<br>
1 2 3<br>
1 4 1<br>
2 4 1<br>
2 3 5<br>
2 7<br>
7 2<br>
1 2<br>
7 7<br>

### Sample Output

9<br>



***
傻逼题系列<br>
给你一个带重边的无向带权图<br>
一个起点和终点<br>
然后每个节点都有一个老司机<br>
第 i 个老司机花费 cost[i] 的代价最远能帮你带 len[i]的距离<br>
然后你每次只能从一个节点上车或下车<br>
问你从起点到终点的最小代价<br><br>

虽然我也不知道为什么被卡了辣么久<br>

先对每个点，根据 len[i] 做一次 Dij 求出多源最短路（神经病啊！）<br>
找出每个路口的老司机能带你去的所有地方<br>
再根据这个连边，cost[i]为权再建一个图<br>
然后再跑一遍 Dij（神经病啊）<br>
然后就好了<br>

<pre><code>
#include &lt;bits/stdc++.h&gt;
#define pb push_back
#define mp make_pair
#define LL long long
using namespace std;

const int INF = 2 * 0x3f3f3f3f;
const int N = 100010;

struct Node
{
    vector<int> V;
    vector<int> W;
};

Node node[3][N];
LL dis[3][N];
int len[N], co[N];
int n, m;
int be, en;


void Dij(int x, int num)
{
    int li;
    priority_queue< pair<int, int> > Q;

    for(int i = 1; i <= n; i ++){
        dis[num][i] = -1;
    }
    dis[num][x] = 0;
    Q.push(mp(0, x));
    while(!Q.empty()){
        int v;

        v = Q.top().second;
        Q.pop();
        li = node[num][v].V.size();
        for(int i = 0; i < li; i ++){
            if(dis[num][node[num][v].V[i]] < 0 || dis[num][node[num][v].V[i]] > dis[num][v] + node[num][v].W[i]){
                dis[num][node[num][v].V[i]] = dis[num][v] + node[num][v].W[i];
                Q.push(mp(-dis[num][node[num][v].V[i]], node[num][v].V[i]));
            }
        }
    }
}


int main()
{
    while(scanf("%d%d", &n, &m) == 2){
        scanf("%d%d", &be, &en);
        for(int i = 1; i <= n; i ++){
            node[0][i].V.clear();
            node[0][i].W.clear();
            node[1][i].V.clear();
            node[1][i].W.clear();
        }
        for(int i = 0; i < m; i ++){
            int u, v, w;

            scanf("%d%d%d", &u, &v, &w);
            node[0][u].V.pb(v);
            node[0][u].W.pb(w);
            node[0][v].V.pb(u);
            node[0][v].W.pb(w);
        }
        for(int i = 1; i <= n; i ++){
            scanf("%d%d", &len[i], &co[i]);
        }
        for(int i = 1; i <= n; i ++){
            Dij(i, 0);
            for(int j = 1; j <= n; j ++) {
                if(i == j){
                    continue;
                }
                if(dis[0][j] >= 0 && dis[0][j] <= len[i]){
                    node[1][i].V.pb(j);
                    node[1][i].W.pb(co[i]);
                }
            }
        }
        Dij(be, 1);
        if(dis[1][en] < 0){
            printf("-1\n");
        }
        else{
            printf("%lld\n", dis[1][en]);
        }
    }

    return 0;
}
