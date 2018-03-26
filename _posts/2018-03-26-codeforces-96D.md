---
layout: post
title: codeforces - 96D
date: 2018-03-26
categories: blog
tags: [ACM,图论,MST]
description: 挡不住风霜
header-img: "img/codeforces96D.jpg"
---




<center><h1><font face="verdana" color="red"> Producing Snow </font></h1></center>

<center><font size="3" face="arial"> Time Limit: 1000MS </font></center>	 
<center><font size="3" face="arial"> Memory Limit: 256 megabytes </font></center>	 	



### Description

Alice likes snow a lot! Unfortunately, this year's winter is already over, and she can't expect to have any more of it. Bob has thus bought her a gift — a large snow maker. He plans to make some amount of snow every day. On day i he will make a pile of snow of volume Vi and put it in her garden.

Each day, every pile will shrink a little due to melting. More precisely, when the temperature on a given day is Ti, each pile will reduce its volume by Ti. If this would reduce the volume of a pile to or below zero, it disappears forever. All snow piles are independent of each other.

Note that the pile made on day i already loses part of its volume on the same day. In an extreme case, this may mean that there are no piles left at the end of a particular day.

You are given the initial pile sizes and the temperature on each day. Determine the total volume of snow melted on each day.

### Input

The first line contains a single integer N (1 ≤ N ≤ 10^5) — the number of days.

The second line contains N integers V1, V2, ..., VN (0 ≤ Vi ≤ 10^9), where Vi is the initial size of a snow pile made on the day i.

The third line contains N integers T1, T2, ..., TN (0 ≤ Ti ≤ 10^9), where Ti is the temperature on the day i.

### Output

Output a single line with N integers, where the i-th integer represents the total volume of snow melted on day i.

### Sample Input

3<br>
10 10 5<br>
5 7 2<br>

### Sample Output

5 12 4<br>



***
傻逼题系列<br>
给你一个带重边的无向带权图<br>
一个起点和终点<br>
然后每个节点都有一个老司机<br>
第 i 个老司机花费 cost[i] 的代价最远能帮你带 len[i]的距离<br>
然后
问你从起点到终点的最小代价<br>
当然


<pre><code>
#include &lt;bits/stdc++.h&gt;
#define LL long long
using namespace std;

const int N = 100010;

int ma[N];
int mb[N];
LL mc[N];
LL cnt1[N];
int cnt2[N];
int n;
int cnt;

int main(int argc, char const *argv[])
{
	while(scanf("%lld", &n) == 1){
	    ma[0] = mb[0] = 0;
	    cnt = 1;
	    memset(cnt1, 0, sizeof(cnt1));
	    memset(cnt2, 0, sizeof(cnt2));
	    for(int i = 1; i <= n; i ++){
	    	scanf("%d", &ma[i]);
    	    }
	    for(int i = 1; i <= n; i ++){
		scanf("%lld", &mb[i]);
		mc[i] = mb[i];
		mc[i] += mc[i - 1];
	    }
	    for(int i = 1; i <= n; i ++){
		int l, r;
		int ans;

		l = i;
		r = n;
		ans = -1;
		while(l <= r){
		    int mid;

                    mid = (l + r) >> 1;
	     	    if(mc[mid] - mc[i - 1] < ma[i]){
                        l = mid + 1;
	    	    }
		    else{
                        r = mid - 1;
                        ans = mid;
	  	    }
		}
		if(ans != -1){
                    cnt1[ans] -= (ma[i] - mc[ans] + mc[i - 1]);
                    cnt2[ans] ++;
		}
	    }
    	    for(int i = 1; i <= n; i ++){
                printf("%lld ", 1ll * cnt * mb[i] - cnt1[i]);
                cnt ++;
                cnt -= cnt2[i];
	    }
	    printf("\n");
	}

	return 0;
}
