---
layout: post
title: 斐波那契数列 - 持续更新
date: 2018-3-8
categories: blog
tags: [ACM,数论]
use_math: true
description: 挡不住风霜。。。
header-img: "img/Fibonacci sequence.jpg"
---

#### 2018-03-08
***

##### 基本介绍

先是斐波那契数列的定义

第一项Fic((0)) = 0，Fic((1)) = 1，以后每一项都是前两项的和<br>
那么有    Fic((n)) = Fic((n - 1)) + Fic((n - 2))<br><br>

所以很显然有一个 O((n)) 的递推<br><br>

然后是有一个通项公式，虽然我目前不知道怎么证明= =!<br>

![](http://www.forkosh.com/mathtex.cgi? Fic(n)=\frac{1}{\sqrt{5}}[(\frac{1+\sqrt{5}}{2})^{n+1}-(\frac{1-\sqrt{5}}{2})^{n+1}]
)

#### 2018-03-11
***

然后现在来说说 O((log(n))) 来求斐波那契数列<br>

建立在快速幂的基础上，<br>

关于快速幂<br>

比如说我们要计算 
$
  \begin{align\*}
    3^{11}
  \end{align\*}
$
<br>直接算，当然是把 3 乘 11 次<br>

很丑陋<br>

我们来把 11 二进制分解
得到 1011

从右往左数第 i 位如果是 1
表示
$
  \begin{align\*}
    3^{2^{i-1}}
  \end{align\*}
$
需要计算在内<br>
而
$
  \begin{align\*}
    3^{1}, 3^{2}, 3^{4}
  \end{align\*}
$
 ……都可以通过自乘得到

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
</code></pre>
矩阵快速幂 
就是用快速幂来加速矩阵乘法= =！

具体的矩阵快速幂和斐波那契数列的关系
我们来看一道题

HDU - 1757


<center><h1><font face="verdana" color="red"> A Simple Math Problem </font></h1></center>

<center><font size="3" face="arial"> Time Limit: 3000/1000 MS (Java/Others) </font></center>	 
<center><font size="3" face="arial"> Memory Limit: 32768/32768 K (Java/Others) </font></center>	 	



### Description

Lele now is thinking about a simple function f(x).

If x < 10 f((x)) = x.
If x >= 10 f((x)) = a0 * f((x-1)) + a1 * f((x-2)) + a2 * f((x-3)) + …… + a9 * f((x-10));<br>
And ai(0<=i<=9) can only be 0 or 1.<br>

Now, I will give a0 ~ a9 and two positive integers k and m ,and could you help Lele to caculate f((k))%m.<br>
### Input

The problem contains mutiple test cases.Please process to the end of file.<br>
In each case, there will be two lines.<br>
In the first line , there are two positive integers k and m. (( k<2*10^9 , m < 10^5 ))<br>
In the second line , there are ten integers represent a0 ~ a9.<br>

### Output

For each case, output f((k)) % m in one line.

### Sample Input

10 9999<br>
1 1 1 1 1 1 1 1 1 1<br>
20 500<br>
1 0 1 0 1 0 1 0 1 0<br>

### Sample Output

45<br>
104<br>



***

其实我觉得只要这张图片就够了

![avater](https://raw.githubusercontent.com/seventeenjcinta/seventeenjcinta.GitHub.io/master/img/hdu1757.jpg)
