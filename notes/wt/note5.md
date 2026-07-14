# 阅读笔记 5

## 阅读范围

本篇主要覆盖论文 Section 7，并结合 Section 10 中关于 working-set heap 的高层设计。前一篇笔记已经整理了下界，本篇重点是看论文如何证明 Dijkstra 加上合适的堆可以匹配时间下界。

Section 10 的数据结构细节比较多，包括 bit vector 和 union-find 的具体维护方式。这里先不展开所有实现细节，而是关注它为什么能提供 Dijkstra 所需的 working-set bound。

## Dijkstra 上界的主线

Section 7 的目标很明确：假设存在一个支持 working-set bound 的堆，那么 Dijkstra 的运行时间可以达到 $O(m+\log D)$。由于 Section 6 已经证明任何算法都需要 $\Omega(m+\log D)$ 时间，这就说明它在时间上达到 universal optimality。

普通 Dijkstra 的扫描边部分本来就是 $O(m)$。如果 insert 和 decrease-key 都是 $O(1)$ 摊还，它们也不会成为瓶颈。需要控制的是所有 delete-min 的总代价。普通 Fibonacci heap 的 delete-min 按当前堆大小计算，因此会得到 $O(n\log n)$ 这一类项；working-set heap 则把删除顶点 $v$ 的代价改成 $O(\log W(v))$，其中 $W(v)$ 表示从 $v$ 插入到被删除期间插入过的顶点数量。

因此 Section 7 的核心问题可以概括为：为什么 Dijkstra 运行过程中所有 $\log W(v)$ 加起来不会超过 $O(\log D)$？

## working-set 和 distance order 的联系

这一部分是论文中比较巧妙的地方。论文把每个顶点在堆中的生命周期看成一个区间：从它被插入堆开始，到它被 delete-min 删除为止。这样每个顶点都有一个区间，working-set size 就对应这个区间中覆盖的插入数量。

接着，论文把这些区间构造成一个 interval DAG，并借用 Van der Hoog 等人的一个引理，得到所有区间长度对数之和可以被这个 DAG 的拓扑序数量控制。再进一步，Dijkstra 运行时形成的 search tree 的拓扑序都是原图的 distance order，因此这个数量又可以和 $D$ 联系起来。

这样就得到关键结论：

$$
\sum_v \log W(v)=O(\log D).
$$

这也回答了前面留下的一个问题：working-set size 的对数之所以和 distance order 的数量有关，是因为 Dijkstra 的堆操作序列不是任意序列，而是由图结构和扫描顺序产生的。图结构允许的 distance order 越少，这些区间也越受限制，delete-min 的总成本就越小。

## Section 10 中堆结构的直觉

Section 7 先假设存在 working-set heap，Section 10 则给出这样的堆结构。这个堆要保留 Fibonacci heap 这类结构的优点：insert 和 decrease-key 是 $O(1)$ 摊还，同时 delete-min 的代价不按整个堆大小计算，而按 working-set size 计算。

从高层看，论文构造了一个“外层堆”，里面维护多个“内层堆”。内层堆可以是 Fibonacci heap 或类似的 fast heap。外层堆按照插入时间把元素分组，较新的元素放在较前面的内层堆里，较旧的元素放在后面。这样，如果一个元素在比较靠后的内层堆中，就说明它插入得比较早，而它前面那些较新的堆里的元素都可以计入它的 working set。

为了让 delete-min 的代价和 working set 对齐，内层堆的大小被控制成大致双指数增长。这样内层堆的编号不会太多，而且可以从编号推出堆大小和 working-set size 之间的关系。至于 decrease-key 需要知道元素在哪个内层堆里，论文用类似 union-find 的结构来维护；找全局最小值则用很短的 bit vector 辅助维护。

## 这一节得到的结论

Section 7 最后得到的结论是：Dijkstra 使用具有 working-set bound 的堆后，时间复杂度是 $O(m+\log D)$，比较次数是 $O(F+\log D)$。前者已经匹配 Section 6 的时间下界；后者和比较次数下界 $O(F-n+1+\log D)$ 还差一个线性项。

因此，论文这一部分的分工比较清楚：

1. Section 7 解决时间最优；
2. Section 10 证明所需的堆确实存在；
3. Section 8 和 Section 9 继续处理比较次数里的线性差距。

## 目前的疑问

Section 7 的整体逻辑比较清楚，但 interval DAG 到 distance order 的映射还需要在最终 review 里解释清楚。否则 $\sum_v \log W(v)=O(\log D)$ 这个结论会显得突然。

Section 10 的实现细节目前可以作为辅助材料处理。最终 review 不一定需要逐行复现所有数据结构维护，更适合重点解释“按插入时间分层、控制内层堆大小、保留 decrease-key”的设计直觉，再选几个关键不变量说明为什么能得到 working-set bound。
