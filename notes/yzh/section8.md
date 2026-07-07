# 第 8 节阅读笔记

## 1. 本节目标

第 7 节已经证明：

* 普通 Dijkstra + working-set heap 的时间是 `O(m + log D)`。
* 但是比较次数还是 `O(F + log D)`。

第 6 节的比较下界是：

* `Ω(F - n + 1 + log D)`。

所以第 8 节要解决的问题是：

* 怎么把普通 Dijkstra 多出来的那个 `n` 级别比较项去掉。

这一节的做法是 Dijkstra with lookahead，也就是单独处理 bottleneck，不再把 bottleneck 都丢进堆里。

## 2. 为什么 bottleneck 重要

第 8 节先说明：如果 bottleneck 不多，那么 `log D` 就会比较大。

Lemma 8.1：

* 如果图里有 `b` 个 bottleneck，那么 `log D >= (n - b) / 2`。

大概思路如下：

* 非 bottleneck 越多，每一层里能换顺序的自由度越多。
* 所以 possible distance order 的数量 `D` 就越大。
* 如果 `D` 已经很大，那么普通 Dijkstra 多出来的比较就可以被 `log D` 吸收。

因此，真正需要特殊处理的是：

* 图里 bottleneck 很多；
* 非 bottleneck 很少；
* `log D` 比 `n` 小很多。

## 3. marked / unmarked bottleneck

第 8 节把 bottleneck 分成两类：

* marked bottleneck：下一层至少有两个顶点，即后面开始分叉了。
* unmarked bottleneck：下一层没有分叉，也就是下一层还是唯一顶点，或者已经是最高层。更像是一条连续窄通道上的点。

Lemma 8.2：

* bottleneck 会支配所有更高层的顶点。
* 从更高层指回 bottleneck 的弧是 useless arc。

Lemma 8.3：

* 如果 `v` 是 unmarked bottleneck，而且不是最高层，那么它可以直接推出下一层唯一顶点的真实距离。

所以连续的 unmarked bottleneck 可以成段处理。

## 4. Dijkstra with lookahead 的直觉

普通 Dijkstra 会把很多顶点放进堆里，再靠 `delete-min` 一个个弹出。但 bottleneck 有特殊结构：

* 它是某一层唯一的点。
* 它支配更高层的点。
* 连续 unmarked bottleneck 的真实距离可以顺着算。

所以第 8 节的这个 lookahead 的想法是：

* 非 bottleneck 还是照常放进堆。
* bottleneck 不进堆，而是放在一个数组 `B` 里单独处理。
* 算法会向前看一段 bottleneck，提前把这一段的真实距离算出来。

## 5. 算法运行流程

预处理：

* BFS 求每个顶点的层数。
* 找出所有 bottleneck。
* 标记 marked bottleneck。

主过程：

* 堆 `H` 只存非 bottleneck。
* 数组 `B` 存当前要处理的一段 bottleneck。
* 列表 `L` 存最终的 true distance order。

每一步比较：

* 堆里当前最小的非 bottleneck；
* `B` 里当前这段 bottleneck。

如果堆里的非 bottleneck 更小，就像普通 Dijkstra 一样处理它。如果 `B` 里的 bottleneck 应该更早进入顺序，就扫描并移动一段 bottleneck 到 `L`。

## 6. 正确性

Theorem 8.4 证明 Dijkstra with lookahead 是正确的。

不变式：

* 顶点被扫描时，当前距离就是真实距离。
* 已经进入 `L` 的顶点，距离不大于还没进入 `L` 的顶点。
* 顶点进入 `L` 的顺序是按真实距离非递减的。

证明的算法直觉：

* bottleneck 支配更高层顶点，所以提前扫描 bottleneck 不会漏掉更短路径。
* 连续 unmarked bottleneck 可以由前一个 bottleneck 推出后一个的真实距离。
* 最后通过父指针 `p(v)` 证明每个点都有前向入弧，所以 `L` 仍然是 distance order。

## 7. 效率

第 8 节要证明：

* 时间：`O(m + log D)`。
* 比较次数：`O(F - n + 1 + log D)`。

几个关键点：

* 只把非 bottleneck 放进堆，所以堆插入次数是 `n - b`。
* 根据 Lemma 8.1，`n - b = O(log D)`。
* 因此非 bottleneck 带来的额外操作可以被 `log D` 控制。

Lemma 8.5：

* `delete-min` 的总代价是 `O(log D)`。
* 分析时可以假装 bottleneck 也做一次插入和立即删除，这样可以继续套用第 7 节 Lemma 7.3 的思路。

Lemma 8.6：

* 在数组 `B` 中搜索的总代价是 `O(log D)`。
* 搜索移动的 bottleneck 段越多，说明可以产生的顺序自由度也越多，因此可以用 `log D` 控制。

最后 Theorem 8.7：

* Dijkstra with lookahead 在 time model 和 comparison model 下都达到 universal optimality。

## 8. 当前理解

第 8 节最重要的直觉是：

* 当 bottleneck 不多时，`log D` 本来就大，普通 Dijkstra 已经够好。
* 当 bottleneck 很多时，这些点结构很特殊，可以不放进堆，而是单独提前处理。

所以 lookahead 的价值在于：它专门处理那些让 `log D` 变小、同时又会让普通 Dijkstra 多做比较的结构。

## 9. 还需要继续确认的问题

* Subcase 2a / 2b 的具体流程还需要对照原文再看一遍。
* Lemma 8.6 里 `B(v)` 集合如何产生多个 distance order，还需要继续理解。