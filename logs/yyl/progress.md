# 过程日志

## 2026-07-07

- 阅读论文 *Universal Optimality of Dijkstra via Beyond-Worst-Case Heaps* 的 Abstract、Introduction 和 Roadmap。
- 确认本文不是提出新的最短路算法，而是重新分析 Dijkstra 在 distance order problem 上的 universal optimality。
- 初步记录 universal optimality 与 worst-case optimality、instance optimality 的区别。
- 关注引言中的链式示例，理解普通 Fibonacci heap 分析为什么不能反映“刚插入就删除”的局部性。

## 2026-07-08

- 阅读 Related Work 和第 3 节，整理 distance order、true distance order、forward arc、dominator、useless arc、level、bottleneck 等定义。
- 对 Lemma 3.1 做了重点标注：一个顶点全序是 distance order 当且仅当除源点外每个顶点都有来自前面顶点的入弧。
- 记录 `D` 表示 distance order 数量，`F` 表示某个 distance order 中前向弧数的最大值。

## 2026-07-09

- 阅读第 4 节，重新理解 Dijkstra 的三个输出：最短距离、最短路树和 true distance order。
- 整理堆实现中 `insert`、`decrease-key`、`delete-min` 对复杂度的影响。
- 理解 Lemma 4.2 中 `F - n + 1` 的来源：`n - 1` 条前向弧负责首次发现顶点，剩余前向弧才可能触发额外比较和 `decrease-key`。

## 2026-07-10

- 阅读第 5-6 节，区分 comparison model 与 time model。
- 整理下界结论：time model 为 `Ω(m + log D)`，comparison model 为 `Ω(F - n + 1 + log D)`。
- 对照 Lemma 6.2 和 Lemma 6.4，确认 `log D` 来自信息论决策树下界，`F - n + 1` 来自前向弧长度扰动的不可区分性。

## 2026-07-11

- 阅读第 7 节，重点理解 working-set heap 如何让普通 Dijkstra 达到 `O(m + log D)` 时间。
- 整理 Lemma 7.3：把每个顶点从插入到删除对应成一个区间，用区间 DAG 的拓扑序数量控制 `sum log W(v)`。
- 初步确认普通 Dijkstra 在时间模型下已 universally optimal，但比较次数仍有一个与 `n` 有关的差距。

## 2026-07-12

- 阅读第 8 节 Dijkstra with lookahead，重点关注 bottleneck 的作用。
- 理解 marked / unmarked bottleneck 的区别：unmarked bottleneck 可以沿连续层提前推导真实距离，marked bottleneck 对应后续分叉。
- 记录算法思路：非 bottleneck 仍放入堆，bottleneck 放入数组 `B` 单独处理，从而把比较次数降到 `O(F - n + 1 + log D)`。

## 2026-07-13

- 阅读第 9 节 recursive Dijkstra，理解它是另一种利用 bottleneck 的方式。
- 整理递归算法的流程：先沿 bottleneck 递归向高层推进，再按相反方向完成各层 Dijkstra run。
- 对比第 8 节和第 9 节：lookahead 更直接，recursive Dijkstra 需要 finger search tree 维护最终顺序。

## 2026-07-14

- 阅读第 10-11 节，了解论文构造 working-set heap 的方式。
- 整理 outer heap / inner heap 的思路：用多个 fast heap 近似维护插入时间顺序，使 `delete-min` 代价和 working-set size 相关。
- 汇总笔记，形成概念、下界、Dijkstra 上界、lookahead / recursive Dijkstra、heap 构造几个部分。
