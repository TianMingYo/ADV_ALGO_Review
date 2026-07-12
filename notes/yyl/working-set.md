# working-set heap 与 Dijkstra 笔记

## 阅读范围

本篇记录第 7 节，重点是为什么带 working-set bound 的 heap 可以让普通 Dijkstra 达到 `O(m + log D)` 时间。

## 分析目标

第 6 节已经给出 time model 的下界 `Ω(m + log D)`。第 7 节要证明的是：如果 Dijkstra 使用满足 working-set bound 的堆，那么运行时间可以达到 `O(m + log D)`。

这里普通 Dijkstra 的贪心框架没有变，真正变化的是堆操作的计费方式。

## working-set heap 的作用

如果堆满足 working-set bound：

- `insert` 是 `O(1)` 摊还。
- `decrease-key` 是 `O(1)` 摊还。
- 删除顶点 `v` 的 `delete-min` 代价是 `O(log W(v))`。

因此，除了遍历弧本身的 `O(m)` 成本之外，关键只剩下所有 `delete-min` 的总代价。

## Lemma 7.3 的理解

Lemma 7.3 要证明：

`sum log W(v) = O(log D)`。

证明思路可以理解为：

- 按顶点插入堆的顺序编号。
- 每个顶点从插入到删除对应一个区间。
- 这个区间长度就是 working-set size。
- 这些区间形成一个 interval DAG。
- interval DAG 的每个拓扑序可以映射到 Dijkstra 搜索树的一个拓扑序。
- 根据 Lemma 3.1，搜索树的拓扑序都是原图的 distance order。

所以，如果很多顶点在堆中等待很久，working-set size 变大，那么图中也会有更多可能的 distance order。这个自由度由 `D` 反映，因此总代价可以被 `log D` 控制。

## Theorem 7.4

Theorem 7.4 得到：

- 有向图时间复杂度：`O(m + log D)`。
- 有向图比较次数：`O(F + log D)`。
- 无向图比较次数：`O(m + log D)`。

这说明普通 Dijkstra 在时间模型下已经 universally optimal。不过比较次数还没有完全匹配第 6 节的下界，因为还差一个和 `n` 有关的加性项。

## 当前理解

第 7 节的核心不是重新证明 Dijkstra 正确，而是把 Dijkstra 产生的堆操作序列和图的 distance order 数量联系起来。working-set heap 能利用操作局部性，而 `D` 则刻画图结构中允许的顺序自由度。
