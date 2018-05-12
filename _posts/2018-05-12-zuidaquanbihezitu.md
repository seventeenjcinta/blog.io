---
layout: post
title: 网络流学习笔记 ———— 最大权闭合子图的证明
date: 2018-05-12
categories: blog
tags: [ACM,图论,网络流]
use_math: true
description: 跟你一样我也留不住她
header-img: "img/Maximumweightedclosedzygogram.jpg"
---


#### 写在前面
***
<br>
感谢《最小割模型在信息学竞赛的应用》，作者：胡伯涛<br>
侵必删，侵必付<br><br><br>

#### 基础内容
***
<br>
首先是闭合子图的概念
> 定义一个有向图 $G=(V,E)$ 的**闭合图**是该有向图的一个点集，且该点集的所有出边都还指向该点集。即闭合图内的任意点的任意后继也一定在闭合图中。更形式化的说，闭合图是这样一个点集 ${V}'\in V$ ，满足对于$\forall \left \langle u,v \right \rangle\in E$ ，若有 $u\in {V}'$ 成立，则必有 $v\in {V}'$ 成立。

用我的话来说，就是你再这个闭合子图中随便找一个点然后往下走，能走到的所有点都在这个闭合子图中。

最大权闭合子图就是字面意思了。<br><br><br>

#### 建图
***
<br>
~~我也不懂为什么要先讲建图再讲证明反正我见到的博客都是这样做的所以我跟着这样做肯定没错~~<br>

将图 $G$ 转化成图 $N=(V_{N},E_{N})$
在原图点集上增加源点 $s$ 和汇点 $t$ <br>
对于所有正权的点 $v$，连接 $s$ 和 $v$，权值为 $w_{v}$<br>
对于所有负权的点 $v$，连接 $v$ 和 $t$，权值为 $-w_{v}$<br>
对于两个相连接的点 $u$，$v$，连接 $u$ 和 $v$，权值为 $inf$<br>

更加形式化的，有：<br>

$$
\begin{align*}
\qquad \qquad \qquad \qquad \qquad \qquad V_{N} &= V\bigcup \left \{ s,t \right \} \\\\
\qquad \qquad \qquad \qquad \qquad \qquad E_{N} &= E\bigcup \left \{ \left \langle s,v \right \rangle |v\in V,w_{v}> 0\right \}\bigcup \left \{ \left \langle v,t \right \rangle |v\in V,w_{v}< 0\right \} \\
\end{align*}
$$

$$
\begin{cases}
c\left ( u,v \right )=\infty & \text{ if } \left \langle s, v \right \rangle \in  E \\ 
c\left ( s,v \right)=w_{v} & \text{ if } w_{v} > 0 \\ 
c\left ( s,v \right)=-w_{v} & \text{ if } w_{v} < 0
\end{cases}
$$

<br><br>

#### 一些概念或者公式或者证明
***

##### 简单割

​	若一个 $s - t$ 割满足割中的每条边都与源点 $s$ 或汇点 $t$ 相关联，称该割为**简单割**（simple cut）。所以在该定义以及上述构图方式下，图 $N$ 的简单割一定不包含图 $G$ 中任意一条边。且最小割为简单割。因为图 $G$ 中的任意一条边在图 $N$ 中权值都是 $inf$ 。



##### 符号规定

​	设简单割 $\left [S,T  \right ]​$ 将图 $N​$ 的点集 $V_{N}​$ 划分成点集 $S_{1}​$ 及其补集 $$
