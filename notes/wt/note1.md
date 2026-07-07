# 阅读笔记 1

## 阅读范围

本篇覆盖论文 *Universal Optimality of Dijkstra via Beyond-Worst-Case Heaps* 的 Abstract、Introduction 和 Roadmap，并对照大作业要求确认阅读重点。证明部分暂时不展开，先看论文的问题意识：它为什么重新讨论 Dijkstra，以及这个经典算法还能从什么角度继续分析。

## 为什么选这篇

Dijkstra 算法本身比较熟悉，作为阅读入口不算太陌生。标题里的 universal optimality 和 beyond-worst-case heaps 则需要额外补背景。这样一来，这篇论文比较适合作为 review 题目：算法入口清楚，但真正的新内容在复杂度分析的角度上。

## 论文想做什么

Abstract 和 Introduction 并不是在介绍一个全新的最短路算法。论文更关心的是：如果 Dijkstra 使用满足 working-set 性质的堆，那么在求 distance order 这个问题上，能不能达到按固定图结构衡量的最优性，也就是 universal optimality。

Roadmap 后面的安排也围绕这条线展开。论文先定义 distance order 和复杂度模型，再证明下界，之后用 Dijkstra 配合合适的 heap 去匹配上界。比较次数还需要额外处理，最后还要真正构造出论文中使用的 heap。

## 初步理解

这里的 universal optimality 不能简单理解成把 $O(m+n\log n)$ 换成另一个复杂度式子。它改变的是分析粒度：先固定一张图，再讨论这张图在不同边权下的最坏情况，而不是只按照 $n$ 和 $m$ 把所有图放在一起分析。

这和常规教材里的 Dijkstra 分析不太一样。教材里通常比较 binary heap、Fibonacci heap 等实现会得到什么复杂度，很少继续追问：两张同样有 $n$ 个点、$m$ 条边的图，结构上可能难度并不一样。

## 对引言例子的理解

引言里的例子主要说明普通堆分析为什么会显得太粗。图中有一条长链，也有一些从源点直接连出去的点。按照普通堆的分析，链上的点被 `delete-min` 时，堆里可能已经有很多较早插入的点，所以每次操作看起来都要付出较大的代价。

但从操作序列看，链上的点通常刚插入不久就会被处理掉，不应该和那些在堆里停留很久的点承担同样的代价。working-set bound 想表达的就是这种差别。Dijkstra 的贪心步骤没有变，变化发生在堆的代价分析上。

## Roadmap 和疑问

Roadmap 给出的路线大致是：先定义 distance order，再建立复杂度模型和下界；时间上界由 Dijkstra 配合 working-set heap 来匹配，比较次数则需要 lookahead 或 recursive Dijkstra 进一步处理。

后续阅读正式定义和证明时，可以先关注几个问题：

1. universal optimality 和 instance optimality 的正式区别是什么？
2. $D$ 和 $F$ 这两个量为什么能进入上下界？
3. working-set size 的对数为什么会和 distance order 的数量有关？
