---
title: Educational Codeforces Round44爆炸记
categories: OI
tags: [信息竞赛,Codeforces]
layout: post
---

在rating=2089的接近橙名时候打了一发Edu Round，结果被虐得超惨……好在没有什么FST导致一发回蓝之类的。而且最后开放Hack的时候叉人是真的爽，也算是没有白掉rating？

### 紧张刺激的WA on pretest

以下题目按照AC顺序列出。阅读时建议跳过题面。

#### [A - Chess Placing](http://codeforces.com/contest/985/problem/A)

> **Statement**
>
> You are given a chessboard of size $1\times n$. It is guaranteed that *n* is even. The chessboard is painted like this: "BWBW...BW".
>
> Some cells of the board are occupied by the chess pieces. Each cell contains no more than one chess piece. It is known that the total number of pieces equals to $\frac{n}{2}$.
>
> In one step you can move one of the pieces one cell to the left or to the right. You cannot move pieces beyond the borders of the board. You also cannot move pieces to the cells that are already occupied.
>
> Your task is to place all the pieces in the cells of the same color using the minimum number of moves (all the pieces must occupy only the black cells or only the white cells after all the moves are made).
>
> **Input**
>
> The first line of the input contains one integer $n$ ($2\le n\le 100$, $n$ is even) — the size of the chessboard.
>
> The second line of the input contains $\frac{n}{2}$ integer numbers $p_1,p_2,\ldots,p_{\frac{n}{2}}$ ($1\le p_i\le n$) — initial positions of the pieces. It is guaranteed that all the positions are distinct.
>
> **Output**
>
> Print one integer — the minimum number of moves you have to make to place all the pieces in the cells of the same color.

一个大水题直接被我想复杂了，用什么贪心模拟……还交了一发FST的假贪心……直接sort以后按顺序把 $a_i$ 与 $2i$(或 $2i-1$)作差的绝对值并求和，然后把与 $2i$ 作的差和与 $2i-1$ 作的差取 $\min$ 就好了。

罚时++

接着为了多捞一些分跳过了B题，毕竟后面的题目掉分快。

#### [C - Liebig's Barrels](http://codeforces.com/contest/985/problem/C)

> **Statement**
>
> You have $m=n\cdot k$ wooden staves. The *i*-th stave has length $a_i$. You have to assemble $n$ barrels consisting of $k$ staves each, you can use any $k$ staves to construct a barrel. Each stave must belong to exactly one barrel.
>
> Let volume $v_j$ of barrel $j$ be equal to the length of the minimal stave in it.
>
> ![img](http://codeforces.com/predownloaded/66/8e/668e3bce94c546efd818de5a228c816594b7cc60.png)
>
> You want to assemble exactly $n$ barrels with the maximal total sum of volumes. But you have to make them equal enough, so a difference between volumes of any pair of the resulting barrels must not exceed $l$, i.e. $\| v_x-v_y\|\le l$ for any $1\le x\le n$ and $1\le y\le n$.
>
> Print maximal total sum of volumes of equal enough barrels or $0$ if it's impossible to satisfy the condition above.
>
> **Input**
>
> The first line contains three space-separated integers $n$, $k$ and $l$ ($1\le n, k\le 10^5$, $1\le n\cdot k\le 10^5$, $0\le l\le 10^9$).
>
> The second line contains $m=n\cdot k$ space-separated integers $a_1, a_2, \ldots, a_m$ ($1\le a_i\le 10^9$) — lengths of staves.
>
> **Output**
>
> Print single integer — maximal total sum of the volumes of barrels or 0 if it's impossible to construct exactly *n* barrels satisfying the condition $\|v_x-v_y\| ≤ l$ for any $1\le x\le n$ and $1\le y\le n$.

一个简单的贪心构造题。

首先判断以下比最短的板子长不超过 $l$ 的板子有多少块，如果少于 $n$ 块直接输出 $0$，否则开始构造：首先拿最小的板子和比最小的板子长不超过 $l$ 的最长的 $n-1$ 块板子初始造 $n$ 个桶，接着从短往长把板子加到短板尽量短的桶里进去，如果一个桶用满了就只好开一个新桶。

结果想了一堆无脑假贪心，为了弥补第一题浪费的时间各种交上去无法过pretest。

罚时+=3

#### [D - Sand Fortress](http://codeforces.com/contest/985/problem/D)

> **Statement**
>
> You are going to the beach with the idea to build the greatest sand castle ever in your head! The beach is not as three-dimensional as you could have imagined, it can be decribed as a line of spots to pile up sand pillars. Spots are numbered $1$ through infinity from left to right.
>
> Obviously, there is not enough sand on the beach, so you brought $n$ packs of sand with you. Let height $h_i$ of the sand pillar on some spot $i$ be the number of sand packs you spent on it. You can't split a sand pack to multiple pillars, all the sand from it should go to a single one. There is a fence of height equal to the height of pillar with $H$ sand packs to the left of the first spot and you should prevent sand from going over it.
>
> Finally you ended up with the following conditions to building the castle:
>
> - $h_1\le H$: no sand from the leftmost spot should go over the fence;
> - For any $i\in [1,\infty)\|h_i-h_i+1\| \le 1$: large difference in heights of two neighboring pillars can lead sand to fall down from the higher one to the lower, you really don't want this to happen;
> - $\sum_{i=1}^{\infty}h_i=n$: you want to spend all the sand you brought with you.
>
> As you have infinite spots to build, it is always possible to come up with some valid castle structure. Though you want the castle to be as compact as possible.
>
> Your task is to calculate the minimum number of spots you can occupy so that all the aforementioned conditions hold.
>
> **Input**
>
> The only line contains two integer numbers $n$ and $H$ ($1\le n, H\le 10^{18}$) — the number of sand packs you have and the height of the fence, respectively.
>
> **Output**
>
> Print the minimum number of spots you can occupy so the all the castle building conditions hold.

一个简单的二分题。最终的最优解形如左边一个直角梯形，右边一个等边直角三角形的形状去掉最上面的一层的一部分（也可能什么都不去掉）。于是直接二分最高点判断两块面积加起来是否大于或等于 $n$ 。注意分类讨论最高层有一个包和有两个包的情况。

结果一开始把 $h_1\le H$ 看成 $\forall i\in[1,\infty),h_i\le H$……

罚时+=3

然后赶紧急急忙忙交上去，结果最高层有两个包的情况没判……

罚时++

剩余时间10min。

#### [B - Switches and Lamps](http://codeforces.com/contest/985/problem/B)

> **Statement**
>
> You are given $n$ switches and $m$ lamps. The $i$-th switch turns on some subset of the lamps. This information is given as the matrix $a$ consisting of $n$ rows and $m$ columns where $a_{i,j}=1$ if the $i$-th switch turns on the $j$-th lamp and $a_{i,j}=0$ if the $i$-th switch is not connected to the $j$-th lamp.
>
> Initially all $m$ lamps are turned off.
>
> Switches change state only from "off" to "on". It means that if you press two or more switches connected to the same lamp then the lamp will be turned on after any of this switches is pressed and will remain its state even if any switch connected to this lamp is pressed afterwards.
>
> It is guaranteed that if you push all $n$ switches then all $m$ lamps will be turned on.
>
> Your think that you have too many switches and you would like to ignore one of them.
>
> Your task is to say if there exists such a switch that if you will ignore (not use) it but press all the other $n-1$ switches then all the $m$ lamps will be turned on.
>
> Input
>
> The first line of the input contains two integers $n$ and $m$ ($1\le n,m\le2000$) — the number of the switches and the number of the lamps.
>
> The following $n$ lines contain $m$ characters each. The character $a_{i,j}$ is equal to '1' if the $i$-th switch turns on the $j$-th lamp and '0' otherwise.
>
> It is guaranteed that if you press all $n$ switches all $m$ lamps will be turned on.
>
> Output
>
> Print "YES" if there is a switch that if you will ignore it and press all the other $n-1$ switches then all $m$ lamps will be turned on. Print "NO" if there is no such switch.

直接开个 $cnt$ 记每个灯被几个开关开，接着枚举每个开关，如果不存在 $cnt=1$ 的灯由它控制，它就是没有用的。

于是拿到了本场第一个一发过pretest。

剩余时间5min。

交上去以后看了一下榜，发现自己居然是交4个题里面罚时最多的之一，感觉很不对劲。即使是我有8个罚时，但是前面还有很多人比我罚时多啊……

然后突然意识到这是Edu Round，罚时算的是所有题目的提交时间和，而不是每个题分别掉分，也就是说我如果先开B题在1.5h前直接A掉我可能也不会这么惨……

然后前面还有一整版的切掉5题的人，两个穿场的，感觉自己凉透了……

然后又看了看Friends Standings，发现同机房里一堆切掉ABCE然后rank比我高的，问了下听说E是个大水题……

比赛结束的那一刻，凌晨时分，整个机房，除了我，都像在聚众吸毒一样兴奋呢。

### 更紧张刺激的深夜Opening Hack

本来对Opening Hack不抱什么希望的。结果我们机房里有个人的E被叉掉了——

整个机房炸锅啦！

原本比赛结束时机房中声浪已是起伏不定，层起迭出，这下却是一浪高过一浪，大有海涌江横之势，完全不像一个凌晨的校园该有的样子。

紧接着过不久又一个人的E也被叉掉了，问了下发现三个人的E的做法是一样的……

于是大家都开始构数据叉最后一个人，要被叉的同学构数据构得最积极。（肥水不流外人田？）

然后我成功地第一个构出了一组数据，并且把它念给了另一个同学让他帮我check，结果一个口滑数据念错，刚发现自己念错的时候机房中传来了清脆的鼠标声——

“啪！”

然后那位要被叉的同学获得了一个Unsuccessful Hack。

在这里对自己念错数据的行为表示道歉并强烈谴责这种听得风是得雨拿别人想出的数据来叉自己的行为。（？）

另：亲手叉掉比自己排名高的队友感觉真好。

于是聚众赌博开始了！

所有人都抱着同一组数据到处叉人，其中有甚欧者，开出不少假代码而叉之，像我这种非酋……

综上所述，Codeforces是一种具有与毒品、赌博性质类似的危害巨大的东西。它具有成瘾性（例如一个人打完一把如果涨了rating就会想再打一把，如果掉了rating就会想再打一把）、致幻性（我今天这把稳了！）、心理伤害性（WA on test 97）、生理伤害性（大半夜不睡觉做什么呢）等诸多特点……

### 余波

第二天再开榜，发现自己的排名涨了一点。抱着对E题的怨念，我坚持着点开一个又一个人的代码，结果点开一份代码以后，一个函数名称赫然刻在屏幕上……

DFS！

什么东西？

DFS！

认真看了一下，似乎是记忆化搜索，但是枚举转移的方式似乎很不优秀，大概算是加了剪枝的记忆化搜索……

于是构了一个极限数据把它卡成平方卡掉了。

接着开始开别人的代码，这么写的居然还不在少数……

于是最后成功拿到了+9。其实还可以叉更多的，只不过叉排名在自己后面的人没啥意义，而且我也累了……

然后看了一下hack记录，居然有一个200+的，几秒能hack一个人，怕不是写了脚本……

### 尾声

rating-=42。

惨。

要是我B题先写应该还不至于这么惨……