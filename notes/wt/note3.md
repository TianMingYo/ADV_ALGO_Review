# 阅读笔记 3

## 阅读范围

本篇覆盖 Section 3 到 Section 5，开始进入正式定义。distance order、true distance order、$D$、$F$、复杂度模型和 universal optimality 这些概念需要先理清楚，后面才能继续看 Section 6 的下界证明。

## distance order 和 true distance order

Section 3 一开始固定一张有向图 $G$ 和源点 $s$，并假设所有点都能从 $s$ 到达。每条边有非负长度，$d^*(v)$ 表示从 $s$ 到 $v$ 的真实最短路距离。

distance order 容易被理解成“按当前边权下的最短路距离排序”。论文里的定义更偏图结构：只要某个顶点顺序可以通过一组非负边权实现为严格递增的最短路距离顺序，它就是一个 distance order。

Lemma 3.1 给了一个直接的判定方式：一个顶点全序是 distance order，当且仅当除了源点以外，每个顶点都有一条从前面顶点指向它的边。这样一来，带权最短路问题的一部分被转成了组合问题：先看图结构本身允许哪些距离顺序。

true distance order 则是给定边权以后才谈的。它要求顺序按真实距离非降排列，同时每个非源点还要有 incoming forward arc。这个额外条件主要是为了处理零长度边；否则所有距离都一样时，单靠“非降”会让太多顺序都合法。

## 对 $D$ 和 $F$ 的理解

$D$ 是图 $G$ 中 distance orders 的数量。这个量的直觉比较清楚：一张图允许的距离顺序越多，算法要区分的情况就越多；如果可能顺序很少，问题自然应该更容易。后面出现 $\log D$，可以先把它看作类似排序问题中的信息量项。

$F$ 的定义更绕一些。它是某个 distance order 里 forward arcs 数量的最大值，而 forward arc 指的是从顺序中较早顶点指向较晚顶点的边。每个非源点至少需要一条 incoming forward arc，所以至少有 $n-1$ 条。论文中出现的 $F-n+1$，可以先理解成“除了基本可达结构以外，多出来的 forward arcs 数量”。

这部分目前还只是直观理解。特别是 $F-n+1$ 为什么会对应比较次数下界，需要等 Section 6 再看。

## Dijkstra 和 distance order 的关系

Section 4 重新表述了一遍 Dijkstra。顶点被分成 unlabeled、labeled 和 scanned 三类，labeled 顶点放在堆里，每次通过 `delete-min` 取出当前距离最小的点。

这里要用到的结论是：Dijkstra 扫描顶点的顺序就是一个 true distance order。有了这一步，论文就可以把分析对象转到 distance order problem 上，而不必一直围绕所有距离值或最短路树展开。

论文还说，如果已经知道 true distance order，那么再恢复真实距离和最短路树不会太贵。这也解释了为什么 distance order problem 可以被单独拿出来分析。不过也要注意，这不等于说 Dijkstra 对所有版本的 SSSP 都已经最优。

## 复杂度模型

Section 5 分了两个模型。comparison model 比较抽象：算法知道图结构，但边权信息只能通过比较来获得，代价主要数比较次数。time model 更接近实际输入：图通过出边表给出，算法访问边和做比较都算时间。

这两个模型最好分开看。前者适合分析“必须比较多少次才能知道顺序”，后者还要考虑算法是不是必须把图的边看一遍。

## universal optimality

到 Section 5，universal optimality 的正式说法清楚了不少。它先固定图 $G$，再看这张图上最坏边权带来的代价，而不是把所有 $n,m$ 相同的图放在一起取最坏情况。

universally optimal 可以先这样理解：对每一张固定图，算法都能达到这张图本身应有的最优最坏情况复杂度。这样看，论文确实是在做 beyond-worst-case 分析，因为它试图区分不同图结构本身的难度。

## 目前的疑问

Section 3 到 Section 5 把问题框架搭起来了，但还没有进入真正的证明。后续看 Section 6 时，可以先关注这几个问题：

1. 为什么任何算法都至少需要 $\Omega(m+\log D)$ 时间？
2. 为什么比较次数下界是 $\Omega(F-n+1+\log D)$？
3. $D$ 和 $F$ 的下界证明分别靠什么构造出来？

如果 Section 6 的下界能看明白，再看 Dijkstra 加 working-set heap 如何匹配上界会更容易。
