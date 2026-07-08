# 阅读笔记 2

## 阅读范围

本篇主要覆盖 Related Work，并补充一些必要背景，顺着 Related Work 往回看，先梳理几条背景线索：Dijkstra 和堆、Fibonacci heap、instance optimality / universal optimality，以及 working-set bound。

## Dijkstra 和堆

Dijkstra 的基本流程比较熟悉：在非负边权下，每次选择当前距离最小的点继续扩展。平时学习这个算法时，堆更像是实现细节；但在这篇论文中，堆的代价直接影响复杂度分析。

如果使用普通堆，复杂度大致是 $O(m\log n)$。Fibonacci heap 的经典作用是把 `decrease-key` 的摊还代价降到 $O(1)$，于是 Dijkstra 可以写成 $O(m+n\log n)$。从这个角度看，heap 会直接影响复杂度表达。

## Fibonacci heap 的局限

Related Work 并不是在否定 Fibonacci heap。它指出的问题是，Fibonacci heap 的分析粒度仍然比较粗，`delete-min` 的代价主要还是看堆的整体规模。

这一点和引言中的例子可以接上。有些顶点刚插入堆，很快就会被删掉；普通分析不会因为这种短停留时间给出更低代价。$O(m+n\log n)$ 很简洁，但它也把许多不同图结构和操作序列压进了同一个最坏情况表达式。

## 两种最优性

instance optimality 和 universal optimality 的差别，可以先从“固定什么”来看。instance optimality 要对每一个具体输入都和最好的算法比较，这个要求很强。对这篇论文讨论的问题来说，如果图和边权都完全固定，理论上可以设计非常针对这个输入的算法，比较起来反而不太自然。

universal optimality 固定的是图结构，而不是完整输入。也就是说，先固定图 $G$，再考虑不同边权。这样一来，算法要适应的是这张图本身的结构难度，而不是只对所有 $n,m$ 相同的图给一个统一上界。

## working-set bound

working-set 这个想法原来更多出现在数据结构里，比如最近访问过的元素应该更容易再次访问。论文把类似想法放到 heap 上：一个元素被 `delete-min` 的代价，除了看当前堆有多大，也看它从插入到删除之间隔了多少次新插入。

这里 `decrease-key` 不能被忽略。Dijkstra 运行时经常会因为找到更短路径而更新堆里的点，所以一个只支持 `insert` 和 `delete-min` 的结构还不够。论文后面要构造真正支持 `decrease-key` 的 working-set heap，Section 10 会专门处理这一点。

## 相关工作线索

Related Work 后面还提到几条线索，包括更快的最短路算法，以及继续讨论 universally optimal Dijkstra 的文章。这些内容目前还不适合展开，后续写 review 的相关工作部分可能用得上。

这里需要注意边界：本文主要讨论 distance order problem。不能简单写成“Dijkstra 对所有单源最短路问题都已经最优”，否则会和后续最短路算法的发展混在一起。

## 目前的问题

背景梳理之后，论文的位置比第一次清楚了一些：它处在最短路算法、堆数据结构和 beyond-worst-case analysis 的交叉处。后续还需要继续弄明白：

1. distance order problem 和普通 SSSP 应该怎么准确区分？
2. universal optimality 的模型里，固定图结构和变化边权具体怎么形式化？
3. working-set heap 的实现为什么要放到论文后面单独证明？
