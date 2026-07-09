# 概念梳理

## universal optimality

本文中的 universal optimality 是一种 beyond-worst-case 分析方式。它固定图结构，但不固定边权；然后比较一个统一算法在该固定图的最坏边权下，是否能达到任何正确算法在同一图上能达到的最优量级。

这和普通 worst-case 不同。普通 worst-case 通常只按 `n` 和 `m` 分类，而这里同样大小的两张图也可能因为结构不同而有不同的难度。

## distance order problem

论文关注的核心问题是 distance order problem，即输出顶点按源点真实距离非递减排列的顺序。Dijkstra 当然能计算最短距离，但本文更强调它的扫描顺序本身也是一个需要分析复杂度的对象。

## distance order

一个顶点全序如果可以由某组非负边权诱导，使得所有真实距离互不相同并严格递增，那么这个顺序就是 distance order。

Lemma 3.1 给出一个重要刻画：一个全序是 distance order，当且仅当除源点 `s` 外，每个顶点 `w` 都有一条来自前面顶点的入弧 `vw`。这个判定把距离顺序问题转化成了图结构问题。

## true distance order

true distance order 是针对某组具体边权而言的。它不仅要按真实距离非递减排列，还必须满足 distance order 的前向入弧条件。

这个定义可以处理零长度边。如果只要求真实距离非递减，那么所有边长为零时任意顺序都合法，这不符合论文要研究的 distance order problem。

## forward arc 与参数 F

给定一个顶点顺序，如果弧 `vw` 的起点 `v` 在终点 `w` 之前，这条弧就是 forward arc。任意 distance order 至少有 `n - 1` 条前向弧，因为每个非源点至少需要一个前向入弧。

论文用 `F` 表示所有 distance order 中前向弧数量的最大值。后面出现的 `F - n + 1` 可以先理解为：去掉每个非源点第一次被发现所必需的 `n - 1` 条前向弧之后，剩余前向弧才可能带来额外比较。

## dominator 与 useless arc

如果从 `s` 到 `w` 的每条路径都经过 `v`，则 `v` dominates `w`。若弧 `vw` 的终点 `w` dominates 起点 `v`，这条弧就是 useless arc。

useless arc 不可能成为任何 distance order 的 forward arc，也不会出现在从源点出发的简单最短路中。论文指出 Dijkstra 实际上会自然忽略这类弧，因此后续算法不需要显式求 dominator tree。

## level 与 bottleneck

顶点 `v` 的 level 是从 `s` 到 `v` 的路径上最少顶点数。某一层如果只有一个顶点，这个顶点就是 bottleneck。

bottleneck 的关键性质是支配所有更高层的顶点。后面优化比较次数时，论文会利用这种结构把 bottleneck 单独处理。

## working-set bound

working-set bound 描述的是堆操作中的局部性。一个顶点如果刚插入堆不久就被删除，它的 `delete-min` 代价应该更小，而不是总按整个堆大小计费。

引言中的例子说明，传统 Fibonacci heap 的 `O(log n)` 删除代价有时不能反映这种局部性。后续阅读需要重点理解论文如何把 working-set size 和图的 distance order 数量联系起来。
