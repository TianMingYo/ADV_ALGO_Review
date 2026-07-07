# 概念梳理

## universal optimality

本文中的 universal optimality 是一种 beyond-worst-case 分析方式。它固定图结构，但不固定边权；然后比较一个统一算法在该固定图的最坏边权下，是否能达到任何正确算法在同一图上能达到的最优量级。

这和普通 worst-case 不同。普通 worst-case 通常只按 `n` 和 `m` 分类，而这里同样大小的两张图也可能因为结构不同而有不同的难度。

## distance order problem

论文关注的核心问题是 distance order problem，即输出顶点按源点真实距离非递减排列的顺序。Dijkstra 当然能计算最短距离，但本文更强调它的扫描顺序本身也是一个需要分析复杂度的对象。

## working-set bound

working-set bound 描述的是堆操作中的局部性。一个顶点如果刚插入堆不久就被删除，它的 `delete-min` 代价应该更小，而不是总按整个堆大小计费。

引言中的例子说明，传统 Fibonacci heap 的 `O(log n)` 删除代价有时不能反映这种局部性。后续阅读需要重点理解论文如何把 working-set size 和图的 distance order 数量联系起来。
