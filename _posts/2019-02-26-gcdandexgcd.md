---
layout: post
title: 数论初步 —— 整除与 gcd
date: 2019-02-25
categories: blog
tags: [ACM,具体数学,学习笔记]
use_math: true	
description: 她再也不会来了
header-img: "https://i.loli.net/2019/02/26/5c74ffe7eb59e.jpg"
---



#### 写在前面

*****

> 笑恨平生不存欢愉

<br><br><br><br>



#### 1、符号定义

***

如果 $m > 0$ 且比值 $n / m$ 是一个整数<br>
我们就说 $m$ 整除 $n$<br>
或者 $n$ 被 $m$ 整除<br>
为了让整除充满仪式感，让我们赋予他一个新的符号，记为：<br>
$$
m \setminus  n \Leftrightarrow  m > 0 \cap \exists k \in \mathbb{Z}, n = mk
$$


<br>其实大多数地方用的是 $m \| n$<br>
不过 $\|$ 在过多地方被涉及（绝对值，集合的定义符，条件概率……）<br>
所以这里换过一个<br>
如果 $m$ 不整除 $n$<br>
就记为 $m \nmid  n$ （因为找不到 Latex反斜线的否定只好丢人的用了$\|$）<br>
然后有两点需要被注意<br>
没有任何数能被 -1 整除<br>
同时这里的讨论是在整数的情况下<br>
<br><br><br><br>

#### 2、最大公因子

------

在上述定义下<br>
那么我们有两个整数 $m$ 和 $n$ 的**最大公因子**（great common divisor）是能整除他们两者的最大整数<br>
$$
gcd(m, n) = max(k | (k \setminus m) \cap (k \setminus n))
$$
<br>皮一下：<br>
$$
gcd(m, n) = max(k | k | m\cap k | n)
$$
<br>我们可以用他来化简一个分数<br>
对于一个分数 $m / n$ 化简为最简有：<br>
$(m / gcd(n, m)) / (n / gcd(n, m))$<br>
所以我们说一个分数 $m / n$ 是最简分数当且仅当 $gcd(n, m) = 1$<br>
同时特殊的，我们有：<br>
$gcd(0, n) = n$<br>
$gcd(0, 0)$ 没有意义<br>
另一个熟悉的符号是**最小公倍数**（least common multiple）<br>
$$
lcm = min(k|k > 0, (m \setminus k) \cap (n \setminus k))
$$
<br>如果 $m \leq 0$ 或者 $n \leq 0$<br>
则 $lcm$ 就没意义<br>
$lcm$ 可以用来对两个分数进行通分<br>
不过我们还是把重点放在 $gcd$ 上<br>
<br><br><br><br>

#### 3、欧几里得算法

------
对于两个数 $n$，$m$ 的 $gcd$<br>
常用方法是用**欧几里得算法**<br>
对给定的 $0\leq m < n$<br>
我们有递归式<br>
$$
gcd(0, n) = n\\
gcd(m, n) = gcd(n \% m, m), m > 0
$$
<br>例如<br>
我们有 $gcd(12, 18) = gcd(6, 12) = gcd(0, 6) = 6$<br>
简单证明一下就是<br>
设 $t$ 为 $n$，$m$ 的公因子<br>
那么我们令<br>
 $n= at$<br>
$m = bt$<br>
那么我们有：<br>
$n \% m = at - \left \lfloor \frac {at}{bt} \right \rfloor bt$<br>
显然依然能被 $t$ 整除<br>
<br><br><br><br>

#### 4、扩展欧几里得算法

------

还有一个基于欧几里得的扩展算法叫做扩展欧几里得<br>
可以用他来计算满足<br>
$$
m'm+n'n = gcd(n, m)
$$
<br>的整数 $m'$ 和 $n'$<br>
我们有：<br>
- 如果 $m = 0$<br>
    那么我们直接取 $m' = 0$，$n' = 1$<br>
- 否则

    我们令 $r = n \%m $，然后用 $(r, m)$ 替换 $(m, n)$ 进行下一轮递归，来计算<br>
    $$
    r''r + m''m = gcd(r, m)
    $$
    <br>

    由于 $r = n - \left \lfloor  \frac{n}{m} \right \rfloor m$ <br>
    之前我们也知道 $gcd(r, m) = gcd(m, n)$<br>
    所以我们有<br>
    $$
    r''(n - \left \lfloor  \frac{n}{m} \right \rfloor m) + m''m = gcd(m, n)
    $$
    然后括号拆开移项一下有：<br>
    $$
    (m'' - \left \lfloor  \frac{n}{m} \right \rfloor r'')m + r''n = gcd(m, n)
    $$



    然后回溯的时候逐项再计算就好了<br>



其实扩展欧几里得首先是在证明欧几里得算法的正确性<br>
假如通过欧几里得算法我们得到了 $gcd(n, m) = d$ <br>
那么我们能确定 $m' $ 和 $n'$ 确定 $m'm + n'n = d$<br>
我们会发现 $m$ 和 $n$ 的公因子必定能整除 $m'm + n'n$<br>
那么他就必定能整除 $d$<br>
即所有公因子 $d' \leq d$<br>
证毕<br>
<br><br><br><br>

#### 5、一个推论

------

从扩展欧几里得中能有一个很简单的推论<br>
$$
k \setminus m \cap k \setminus n \Leftrightarrow  k\setminus gcd(m, n)
$$
<br>

证明很简单，只需要感性认知<br>
那么用处也很简单<br>
假如有一天我们需要对 $n$ 的所有因子求和<br>
那么我们有<br>
$$
\sum_{m\setminus n}a_m = \sum_{m\setminus n}a_{n / m}
$$
<br>可以注意下斜线的方向= =！<br>
这个等式也很好理解<br>
首先因子是成对存在的<br>
即当你 $m$ 遍历 $n$ 的所有因子的时候<br>
你 $n / m$ 也在遍历所有的因子<br>
更一般的<br>
我们有：<br>
$$
\sum_{m\setminus n}a_m = \sum_{k}\sum_{m > 0}a_m[n = mk]
$$
<br>可以把把 $k$ 当成在从 1 开始枚举因子<br>
那么对于因子求和的二重式我们有<br>
$$
\sum_{m \setminus n} \sum_{k\setminus m} a_{k, m} = \sum_{k \setminus n} \sum_{l \setminus (n / k)} a_{k, kl}
$$
<br>一个 $n = 6$ 的例子就可以很清楚的看懂了：<br>
$$
ans=(a_{1,1}) + (a_{1, 2} + a_{2, 2}) + (a_{1, 3} + a_{3, 3}) + (a_{1, 6} + a_{2, 6} + a_{3, 6} + a_{6, 6})
$$
<br>另一方面<br>
交换求和方式后我们有：<br>
$$
ans=(a_{1,1} + a_{1, 2} + a_{1, 3} + a_{1, 6}) + (a_{2, 2} + a_{2, 6}) + (a_{3, 3} + a_{3, 6}) + (a_{6, 6})
$$
<br>

有一个证明和式很好的方法叫做**艾弗森约定**<br>
即 $\sum_{k}a_k[p(k)]$<br>
其中 $p(k)$ 为对 $k$ 的正确与否的判断<br>
对于上述求和式<br>
左边有：<br>
$$
\sum_{j}\sum_{k, l}a_{k, m}[n = jm][m = kl] = \sum_{j}\sum_{k, l}a_{k, kl}[n = jkl]
$$
<br>右边有：<br>
$$
\sum_{m}\sum_{l, k}a_{k, kl}[n = jk][n / k = ml] = \sum_{m}\sum_{l, k}a_{k, kl}[n = mlk]
$$
除了变量名不一样以外其余的都相同<br><br><br><br>

#### 6、来做题吧

------

HDU - 5528<br>
题解 - http://seventeenjcinta.com/blog/2018/09/09/hdu-5528/<br>
CF - 787A<br>
题解 - 没有<br>
