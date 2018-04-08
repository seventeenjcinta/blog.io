---
layout: post
title: 监狱求生
date: 2018-04-08
categories: blog
tags: [ACM,数学,转载]
use_math: true
description: 挡不住风霜。。。
header-img: "img/jianyuqiusheng.jpg"
---

昨天在写大连海事大学校赛的时候<br>
发现一题<br>
原型在 gls 知乎专栏里看到过<br>
当时看到就觉得很有意思<br>
趁这个机会<br>
在 gls 的同意下把那个专栏的内容转载过来<br>
感谢 gls @好地方bug<br>
祝 gls 和 清欢 百年好合<br>



##### 一个有趣的问题
有100个犯人被关在监狱里，邪恶的监狱长构思了一个处决犯人的计划。监狱长准备了100个盒子，每个盒子里面装有一个犯人的名字。他将这100个盒子排成一排，放在一个房间里面。一开始，所有犯人都在房间的外面，然后依次进入房间，每位进入房间的犯人可以开启50个盒子，然后关上。在所有犯人都进入房间后，如果有哪个犯人没有看到自己的名字，那所有的犯人都会被处以极刑。

请问如何制定一个策略，使得犯人们的存活概率最大

注意：在邪恶监狱长的计划开始后，每位犯人都不能从其他犯人那里得到任何信息（比如前面的人有没有看到自己的名字）。




##### 不采取任何策略
首先考虑犯人们不采取任何策略，这样以来，每位犯人看到自己的名字的概率将会是 1/2 ，所以总得存活概率就是 $(1/2)^{100}$ ，显然这基本等于不可能了。。



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
