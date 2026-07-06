# 第 7 节阅读笔记

## 1. 本节目标

第 6 节已经证明了 time model 的下界是 `Ω(m + log D)`。第 7 节要证明的是：

* Dijkstra 如果用满足 working-set bound 的堆实现，运行时间可以做到 `O(m + log D)`。
* 所以普通 Dijkstra 在时间意义下已经是 universally optimal 的。

但是比较次数还没有完全匹配下界。

* 第 7 节得到的比较次数是 `O(F + log D)`。
* 第 6 节的比较下界是 `Ω(F - n + 1 + log D)`。
* 这里还差一个和 `n` 有关的加性项，所以后面第 8、9 节还要继续改算法。

## 2. working-set heap 在这里的作用

如果堆有 working-set bound：

* `insert` 是 `O(1)` 摊还。
* `decrease-key` 是 `O(1)` 摊还。
* `delete-min` 删除顶点 `v` 的代价是 `O(log W(v))`。

所以 Dijkstra 的主要问题变成：

* 除了堆操作之外，本来就是 `O(m)`。
* `insert` 和 `decrease-key` 也都比较便宜。
* 真正需要分析的是所有 `delete-min` 的总代价。

也就是说，关键是证明：

`sum log W(v) = O(log D)`

## 3. Lemma 7.1 的作用

Lemma 7.1 是从别的论文引用来的区间 DAG 结论。考虑如下思路：

* 每个区间可以看成一个对象。
* 如果一个区间完全在另一个区间前面，就形成一个先后约束。
* 这些约束构成一个 DAG。
* 如果区间普遍很长，说明可交换的顺序可能更多。
* 所以区间长度的对数和，可以被拓扑序数量的对数控制。

这一步主要是一个桥梁：把 working-set size 变成区间长度，再和 distance order 的数量 `D` 联系起来。

## 4. Lemma 7.3

提出`sum log W(v) = O(log D)`

证明的思路大概是：

* 按顶点被插入堆的顺序编号。
* 对每个顶点，从它被插入堆到被删除，中间插入过的顶点形成一个区间。
* 这个区间长度就是 working-set size。
* 用这些区间构造一个区间 DAG。
* 区间 DAG 的每个拓扑序，都能对应到 Dijkstra 搜索树的一个拓扑序。
* 根据 Lemma 3.1，搜索树的拓扑序都是原图的 distance order。
* 所以原图的 distance order 数量 `D` 至少能控制这些区间 DAG 的拓扑序数量。
* 再用 Lemma 7.1，就得到 `sum log W(v) = O(log D)`。

如果很多顶点在堆里等得很久，working-set size 很大，那么说明运行过程中存在更多可能的顺序自由度，所以 `D` 也会变大。

## 5. Theorem 7.4

最后得到：

* 有向图上时间复杂度：`O(m + log D)`。
* 有向图上比较次数：`O(F + log D)`。
* 无向图上比较次数：`O(m + log D)`。

所以：

* 时间复杂度已经匹配第 6 节下界。
* 比较次数还没有完全匹配。

这解释了为什么后面还需要 Dijkstra with lookahead 和 recursive Dijkstra。

## 6. 当前理解

第 7 节的核心不是重新证明 Dijkstra 正确，而是证明 Dijkstra 的堆操作代价可以被 `log D` 控制。

前面第 6 节给的是下界，第 7 节给的是第一个上界。到这里为止，论文已经证明普通 Dijkstra 在时间模型下是 universal optimal 的。

## 7. 还需要继续确认的问题

* Lemma 7.1 的区间 DAG 结论本身没有证明，只是引用结果，暂时先接受。
* Lemma 7.3 中“区间 DAG 的拓扑序对应搜索树拓扑序”这一点还可以再画图理解。
* 为什么比较次数差的那个 `n` 项，需要第 8、9 节来补，还要继续看。
