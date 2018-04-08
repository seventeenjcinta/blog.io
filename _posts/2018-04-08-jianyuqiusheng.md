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
首先考虑犯人们不采取任何策略，这样以来，每位犯人看到自己的名字的概率将会是 1/2 ，所以总的存活概率就是 $(1/2)^{100}$ ，显然这基本等于不可能了。。




##### 一个简单的改进
不妨按照进入房间的顺寻将犯人编号，从1到100。虽然每个犯人不能从前面的犯人那里得到任何信息，但是自己要活下来，必须先假设其他人都看到了对应的名字，所以容易想到一个简单的改进：

- 奇数编号的人都开前50个盒子
- 偶数编号的人都开前50个盒子
让我们来考虑一下这时候，前两个人存活的概率，是不是比 1/4 更小。令事件 A 表示两个人都活下来，事件$ B_1 $是第一个人看到自己的名字，事件$ B_2 $是第二个人看到自己的名字。显然， A 应当是$ B_1 $和$ B_2 $联合事件。容易知道$ B_1 $和$ B_2 $并不独立，这是因为，**如果第一个人在前50个盒子里面看到了自己的名字，那么第二个人就不可能看到第一个人的名字**，换言之，在假设第一个人能看到自己名字的情况下，第二个人相当于在99个盒子里面抽50个盒子。那么我们有: $$ P\left \{ A \right \} = P\left \{ B_1 \right \}P\left \{ B_2|B_1 \right \} = \frac{1}{2}\times \frac{59}{99} = \frac{25}{99}< \frac{1}{4} $$ 两两一组，整体活下来的概率就是$ (\frac{25}{99})^{50} $，比不采取任何策略要好很多了，但是还是依然处于不可能的阶段。