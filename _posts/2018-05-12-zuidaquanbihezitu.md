---
layout: post
title: 最大权闭合子图 —— 学习笔记
date: 2018-05-12
categories: blog
tags: [ACM,图论,网络流,学习笔记]
use_math: true
description: 跟你一样我也留不住她
header-img: "https://i.loli.net/2018/09/18/5ba097ee27b82.jpg"
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
c\left ( v,t \right)=-w_{v} & \text{ if } w_{v} < 0
\end{cases}
$$

<br><br>

#### 一些概念或者公式或者证明
***

##### 简单割

​	若一个 $s - t$ 割满足割中的每条边都与源点 $s$ 或汇点 $t$ 相关联，称该割为**简单割**（simple cut）。所以在该定义以及上述构图方式下，图 $N$ 的简单割一定不包含图 $G$ 中任意一条边。且最小割为简单割。因为图 $G$ 中的任意一条边在图 $N$ 中权值都是 $inf$ 。



##### 符号规定

​	设简单割 $\left [S,T  \right ]$ 将图 $N$ 的点集 $V_{N}$ 划分成点集 $S$ 及其补集 $T(T = V_{N} - S)$ ，满足 $s\in S$ ，$t \in T$ 。
	设 $G$ 中的一个闭合图为 $V_{1}$，它在 $G$ 中的补集为 $V_{2}(V_{2} = V - V_{1})$。
	记 $V^{+}$ 为 $V$ 中点权为正的点集，$V^{-}$ 为 $V$ 中点权为负的点集。
	同样的可以定义$V_{1}^{+}$ ，$V_{1}^{-}$，$V_{2}^{+}$，$V_{2}^{-}$ 。



##### 一个结论及其证明

​	在简单割和闭合子图的一一对应下，我们有：<br><br>
$$
\qquad \qquad \qquad \qquad \qquad \qquad c\left [ S,T\right ] = \sum_{v\in V_{2}^{+}}w_{v} + \sum_{v\in V_{1}^{-}}-w_{v}
$$<br>
​	证明：<br>

​		对于图中的和闭合子图对应的一个割 $\left [S,T  \right ]$ 按照与源点，汇点的连接情况，可以分成三个部分：<br><br>
$$
\qquad \qquad \qquad \qquad \qquad \qquad  \left [ S,T\right ] = \left [ \left \{ s \right \},V_{2} \right ]\bigcup \left [ V_{1},\left \{ t \right \} \right ] \bigcup\left [ V_{1},V_{2}\right ]
$$<br><br>
​		由于 $\left [S,T  \right ]$ 是一个简单割，所以有 $\left [V_{1},V_{2}  \right ] = \varnothing$<br>
​		因为源点 $s$ 只与 $V^{+}$ 相连，所以有 $$\left [ \left \{ s \right \},V_{2} \right ]=\left [ \left \{ s \right \},V_{2}^{+} \right ]$$ <br>
​		因为汇点 $t$ 只与 $V^{-}$ 相连，所以有 $$\left [ V_{1},\left \{ t \right \} \right ]=\left [ V_{1}^{-},\left \{ t \right \} \right ]$$ <br>
		 综上有 $$\left [ S,T\right ] = \left [ \left \{ s \right \},V_{2}^{+} \right ]\bigcup \left [ V_{1}^{-},\left \{ t \right \} \right ]$$<br>

​	证毕。<br>



##### 另一个结论及其证明

​	最优性：当图 $N$ 取到最小割的时候，对应的图 $G$ 的闭合图达到最大权



​	证明：

​		对于闭合图 $V_{1}$，有：<br>
$$
\qquad \qquad \qquad \qquad \qquad \qquad w(V_{1}) = \sum_{v\in V_{1}^{+}}w_{v} - \sum_{v\in V_{1}^{-}}-w_{v}
$$<br><br>
​		然后我们把这个式子和上面证明的那个式子~~（我就是不想对公式标号你不服来打我啊）~~联立：<br><br>
$$
\begin{align*}
\qquad \qquad \qquad \qquad \qquad \qquad w\left ( V_{1} \right )+c\left [ s,t \right ] &= \sum_{v\in V_{1}^{+}}w_{v} - \sum_{v\in V_{1}^{-}}-w_{v} + \sum_{v\in V_{2}^{+}}w_{v} + \sum_{v\in V_{1}^{-}}-w_{v} \\
&= \sum_{v\in V_{1}^{+}}w_{v} + \sum_{v\in V_{2}^{+}}+w_{v} \\
&= \sum_{v\in V^{+}}w_{v}
\end{align*}
$$<br>
​		整理一下有：<br><br>
$$
\qquad \qquad \qquad \qquad \qquad \qquad w\left ( V_{1} \right ) = \sum_{v\in V^{+}}w_{v} -c\left [ s,t \right ]
$$<br><br>
​	所以我们的目标是最大化 $w\left ( V_{1} \right )$，而正权点的权值总和 $\sum_{v\in V^{+}}w_{v}$ 为一个定值，所以就等价于求原图最小割，然后就可以转化成最大流了。<br><br><br>

#### 结论
***
<br>

最大点权和 = 正权点和 - 最小割

