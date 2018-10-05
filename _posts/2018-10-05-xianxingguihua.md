---
layout: post
title: 线性规划学习笔记
date: 2018-10-05
categories: blog
tags: [ACM,思维,学习笔记]
use_math: true	
description: 她再也不会来了
header-img: "https://i.loli.net/2018/10/05/5bb7126a99768.jpg"
---

#### 写在前面

---

<br>

> 我将违背我的天性，忤逆我的本能，永远爱你

<br><br><br>

#### 线性规划概念

---

<br>

线性规划是一种最优化问题<br>
在定义域中求解目标函数((objective function)) 的最大值最小值问题<br>
**目标函数必须是线性的**<br>
而定义域是由一系列线性约束确定的<br>
若一个函数 $f​$ 是线性的那么我们有 $f(c_1x_1 + c_2x_2) = c_1 f(x_1) + c_2 f(x_2)​$
让我们来举个栗子<br>

$$
\begin{eqnarray}\underset{x_1\geqslant 0, x_2 \leqslant  0, x_3}{max}  v_1x_1+ v_2x_2 + v_3x_3\\
s.t.  \qquad a_1x_1 + x_2 + x_3\leqslant  b_1\\
x_1 + a_2x_2 = b_2 \\
a_3x_3\geqslant b_3
\end{eqnarray}
$$
其中变量有 $x_1, x_2, x_3$<br>
常数有 $v, a, b$<br>
这个栗子是要最大化<br>
每个约束左边是一个线性方程<br>
右边是一个常数<br>
同时线性规划过程中不能出现严格不等式，即不能出现小于号 or 大于号<br>
限制每个变量是非负、非正或者无限制((unrestricted))的约束条件，称为特殊((special))约束<br>
这种约束一般会列在 $max$ 或 $min$ 下面<br>



#### 对偶问题

---

<br>

上面的栗子我们一般叫做**原问题**<br>
对于任意一个线性规划来说，都有一个与之相关联的对偶问题((dual))<br>
接下来是转化对偶问题的步骤<br>
- 将目标函数改写为最小化问题<br>

  ​
  $$
  \underset{x_1\geqslant 0, x_2 \leqslant  0, x_3}{min} - v_1x_1 - v_2x_2 - v_3x_3
  $$
  
  如果一个解能最大化该目标函数，也就能最小化该目标函数的取反值，所以并不会影响到最终的解集


- 将不等式约束改写为“小于等于”的形式，且将每个约束条件的常数移项，使得式子右边为 0

  ​
  $$
  \begin{eqnarray}\underset{x_1\geqslant 0, x_2 \leqslant  0, x_3}{min}  -v_1x_1 - v_2x_2 - v_3x_3\\
  s.t.  \qquad a_1x_1 + x_2 + x_3 - b_1\leqslant 0\\
  x_1 + a_2x_2 - b_2 = 0 \\
  -a_3x_3 + b_3\leqslant 0
  \end{eqnarray}
  $$







- 给每个不等式约束定义对应的非负对偶变量，给每个等式约束定义无约束的对偶变量

  ​

  对于 $ a_1x_1 + x_2 + x_3 - b_1\leqslant 0$ 增加约束 $\lambda_1 \geqslant 0$<br>
  对于 $a_3x_3 - b_3\leqslant 0$ 增加约束 $\lambda_3 \geqslant 0$<br>
  对于 $x_1 + a_2x_2 - b_2 = 0$ 增加约束 $\lambda_2$<br>

- 移除每个约束条件，并将((对偶变量))∗((约束条件的左边))**加**到目标函数中，并将对偶变量作为**新的变量**构造一个最大化问题。

  ​

  例如我们把 $ a_1x_1 + x_2 + x_3 - b_1\leqslant 0$  移除后<br>
  我们应该把 $\lambda_1( a_1x_1 + x_2 + x_3 - b_1\leqslant 0)$ **加**到目标函数上
  然后我们的式子就变成了这样<br>
  $$
  \begin{eqnarray}	\underset{\lambda_1 \geqslant 0, \lambda_2, \lambda_3 \geqslant  0}{max}\quad \underset{x_1\geqslant 0, x_2 \leqslant  0, x_3}{min}  -v_1x_1 - v_2x_2 - v_3x_3\\ + \lambda_1( a_1x_1 + x_2 + x_3 - b_1)\\
  +\lambda_2( x_1 +a_2 x_2  - b_2)\\
  +\lambda_3( -a_3x_3 + b_3)\\
  \end{eqnarray}
  $$

- 原目标函数中的项为 ((**对偶变量**))∗((带有**原变量**的表达式)) + 其他只与**原变量**有关的项，我们把它改成((**原变量**))∗((带有**对偶变量**的表达式)) + 其他只与**对偶变量**有关的项<br>

  ​
  $$
  \begin{eqnarray}	\underset{\lambda_1 \geqslant 0, \lambda_2, \lambda_3 \geqslant  0}{max}\quad \underset{x_1\geqslant 0, x_2 \leqslant  0, x_3}{min}  -b_1\lambda_1 - b_2\lambda_2 + b_3\lambda_3\\
  +x_1( a_1\lambda_1 + \lambda_2 - v_1)\\
  +x_2( \lambda_1 +a_2 \lambda_2  - v_2)\\
  +x_3(\lambda_1 -a_3\lambda_3 - v_3)\\
  \end{eqnarray}
  $$



- 移除形如 ((原变量))∗((带有对偶变量的表达式))的项，并按照下述规则添加新的约束条件

  ​

  - 当原变量为非负，表达式大于等于 0
  - 当原变量为非正，表达式小于等于 0
  - 当原变量无限制，表达式等于 0
    $$
    \begin{eqnarray}\underset{\lambda_1 \geqslant 0, \lambda_2, \lambda_3 \geqslant  0}{max} -b_1\lambda_1 - b_2\lambda_2 + b_3\lambda_3\\
    s.t.  \qquad
    a_1\lambda_1 + \lambda_2 \geqslant v_1\\
     \lambda_1 +a_2 \lambda_2  \leqslant v_2\\
     \lambda_1 -a_3\lambda_3 = v_3\\
    \end{eqnarray}
    $$







- 如果在第一步时将问题改写成了最小化问题，那么现在将上一步得到的结果改写为最小化问题。否则跳过此步

  ​
  $$
  \begin{eqnarray}\underset{\lambda_1 \geqslant 0, \lambda_2, \lambda_3 \geqslant  0}{min} b_1\lambda_1 + b_2\lambda_2 - b_3\lambda_3\\
  s.t.  \qquad
  a_1\lambda_1 + \lambda_2 \geqslant v_1\\
   \lambda_1 +a_2 \lambda_2  \leqslant v_2\\
   \lambda_1 -a_3\lambda_3 = v_3\\
  \end{eqnarray}
  $$
  <br><br><br>

#### 一个结论

*******

如果一个线性规划问题有最优解，那么它的对偶问题也有，且解相等

