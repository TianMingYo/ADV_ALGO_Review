# Dijkstra 部分笔记

## 第 4 节阅读范围

本篇记录第 4 节对 Dijkstra 算法的重新理解，重点是它和 true distance order 的关系，以及堆实现中比较次数的来源。

## Dijkstra 输出的三个对象

第 4 节强调，Dijkstra 实际上完成三件事：

1. 计算源点到所有顶点的真实距离。
2. 维护父指针，得到一棵最短路树。
3. 按扫描顺序得到一个 true distance order。

Lemma 4.1 证明 Dijkstra 的扫描顺序是 true distance order。原因是每个非源点在被扫描前一定先被某条弧发现，发现它的顶点已经排在它之前，因此满足 Lemma 3.1 的前向入弧条件。

## 堆实现

用堆实现 Dijkstra 时，堆中保存 labeled 但尚未 scanned 的顶点，key 是当前距离。

- 新发现顶点时执行 `insert`。
- 已发现顶点的当前距离变小时执行 `decrease-key`。
- 每轮通过 `delete-min` 取出下一个要扫描的顶点。

因此，除遍历弧之外，复杂度主要来自堆操作。普通分析会把 `delete-min` 按堆大小计费，而本文后续会用 working-set bound 做更细的分析。

## Lemma 4.2 的理解

Lemma 4.2 表明，Dijkstra 运行中的堆外比较次数和 `decrease-key` 次数都至多是 `F - n + 1`。

这里的直觉是：

- Dijkstra 扫描顺序本身是一个 distance order。
- 每次对 labeled 顶点做松弛比较时，对应的弧一定是这个顺序中的 forward arc。
- 其中 `n - 1` 条前向弧负责第一次发现非源点，只触发 `insert`，不触发额外比较。
- 剩余的前向弧才可能触发堆外比较和 `decrease-key`。

所以 `F - n + 1` 是连接 Dijkstra 操作次数和图结构参数的重要量。

## 当前理解

第 4 节的作用是把“求最短路”转化成“求 true distance order”。后面第 5、6 节会给这个问题建立下界，第 7 节之后再证明 Dijkstra 及其变体可以匹配这些下界。
