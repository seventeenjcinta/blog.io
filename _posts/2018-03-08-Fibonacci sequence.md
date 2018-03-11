---
layout: post
title: 斐波那契数列 - 持续更新
date: 2018-3-8
categories: blog
tags: [ACM,数学]
use_math: true
description: 挡不住风霜。。。
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

#### 2018-03-11
***

然后现在来说说 O(log(n)) 来求斐波那契数列<br>

建立在快速幂的基础上，<br>

关于快速幂<br>

比如说我们要计算 
&
3^{11} 
&
直接算，当然是把 3 乘 11 次<br>

很丑陋<br>
   
我们来把 11 二进制分解
得到 1011

从右往左数第 i 位如果是 1
表示& 3^{2^{i-1}} & 需要计算在内
而 & 3^{1} &, & 3^{2} &, & 3^{4} & ……都可以通过自乘得到

这样就成了 log 级别了！
代码也很短

<pre><code>

LL quickmod(LL a, LL b, LL MOD)
{
    LL ans = 1;
    while(b){
        if(b & 1){
            ans = (ans * a) % MOD;
        }
        b >>= 1;
        a = a * a % MOD;
    }

    return ans % MOD;
}
