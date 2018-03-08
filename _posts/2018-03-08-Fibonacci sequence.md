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
那么有    Fic(n) = F(n - 1) + F(n - 2)<br><br>

所以很显然有一个 O(n) 的递推<br><br>

然后是有一个通项公式，虽然我目前不知道怎么证明= =!<br>

![](http://www.forkosh.com/mathtex.cgi? Fic(n)=\frac{1}{\sqrt{5}}[(\frac{1+\sqrt{5}}{2})^{n+1}-(\frac{1-\sqrt{5}}{2})^{n+1}]
)
