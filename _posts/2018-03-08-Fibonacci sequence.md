---
layout: post
title: 斐波那契数列 - 持续更新
date: 2018-3-8
categories: blog
tags: [ACM,数学]
description: 挡不住风霜。
header-img: "img/Fibonacci sequence.jpg"
---

#### 2018-03-08
***

##### 基本介绍

先是斐波那契数列的定义

第一项Fic(0) = 0，Fic(1) = 1，以后每一项都是前两项的和<br>
那么有    Fic(n) = icF(n - 1) + Fic(n - 2)<br><br>

所以很显然有一个 O(n) 的递推<br><br>

然后是有一个通项公式，虽然我目前不知道怎么证明= =!<br>

![](http://www.forkosh.com/mathtex.cgi? Fic(n)=\frac{1}{\sqrt{5}}[(\frac{1+\sqrt{5}}{2})^{n+1}-(\frac{1-\sqrt{5}}{2})^{n+1}]
)

#### 2018-03-09
***

然后现在来说说 O(log(n)) 来求斐波那契数列<br>

建立在快速幂的基础上<br>

关于快速幂<br>



很显然是我们可以把 2 乘 13 次<br>


很丑陋<br>



其实我们可以计算 <a href="https://www.codecogs.com/eqnedit.php?latex=2^{1}&space;*&space;2^{4}&space;*&space;2^{8}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?2^{1}&space;*&space;2^{4}&space;*&space;2^{8}" title="2^{1} * 2^{4} * 2^{8}" /></a>

