# Dijkstra with lookahead 笔记

## 阅读范围

本篇记录第 8 节 Dijkstra with lookahead。第 7 节已经说明普通 Dijkstra 在时间上是 universally optimal，但比较次数仍是 `O(F + log D)`，还没有完全匹配 `Ω(F - n + 1 + log D)`。

## 为什么 bottleneck 重要

Lemma 8.1 表明，如果图中有 `b` 个 bottleneck，则：

`log D >= (n - b) / 2`。

也就是说，如果 bottleneck 不多，那么 `log D` 本身就足够大，可以吸收普通 Dijkstra 多出来的比较项。真正需要特殊处理的是 bottleneck 很多、`log D` 相对较小的图。

## marked 与 unmarked bottleneck

bottleneck 是某一层唯一的顶点。

- marked bottleneck：下一层至少有两个顶点，意味着后面开始分叉。
- unmarked bottleneck：下一层仍然没有分叉，或者已经是最高层。

Lemma 8.2 说明 bottleneck 支配所有更高层的顶点。Lemma 8.3 说明，如果 `v` 是非最高层的 unmarked bottleneck，则下一层唯一顶点 `w` 的真实距离可以通过某条弧 `vw` 从 `v` 推出。

## 算法直觉

Dijkstra with lookahead 的做法是：

- 非 bottleneck 仍然进入 heap。
- bottleneck 不进入 heap，而是放在数组 `B` 中单独处理。
- 算法先用 BFS 求 level，找出 bottleneck，并标记 marked bottleneck。
- 主循环比较 heap 中最小非 bottleneck 和当前 `B` 段中的 bottleneck。

如果 heap 顶点更小，就像普通 Dijkstra 一样扫描它。如果 bottleneck 应该先进入顺序，就扫描并移动一段 bottleneck 到输出列表 `L`。

## 正确性直觉

正确性的关键不变式是：

- 顶点被扫描时，当前距离等于真实距离。
- 已经加入 `L` 的顶点距离不大于还没加入 `L` 的顶点。
- 顶点按真实距离非递减加入 `L`。

bottleneck 支配更高层顶点，所以提前处理 bottleneck 不会漏掉从源点到更高层的必要路径。连续 unmarked bottleneck 的真实距离又可以沿层级逐步推出。

## 效率结论

Theorem 8.7 得到：

- 时间复杂度：`O(m + log D)`。
- 有向图比较次数：`O(F - n + 1 + log D)`。
- 无向图比较次数：`O(m - n + 1 + log D)`。

因此 Dijkstra with lookahead 同时达到时间和比较意义下的 universal optimality。

## 当前理解

lookahead 的价值在于：当 bottleneck 很多时，普通 Dijkstra 会为这些结构上几乎被迫出现的顶点付出不必要的堆比较。把 bottleneck 单独处理之后，算法只把真正需要比较排序的非 bottleneck 放进堆，从而补上第 7 节留下的比较次数缺口。
