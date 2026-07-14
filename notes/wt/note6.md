# 阅读笔记 6

## 阅读范围

本篇覆盖论文 Section 8、Section 9 和最后的 Remarks。前一篇笔记已经看到，普通 Dijkstra 加 working-set heap 在时间上达到最优，但比较次数仍然是 $O(F+\log D)$，和下界 $\Omega(F-n+1+\log D)$ 之间差了一个线性项。本篇重点是看论文如何进一步消除这个差距，并整理对整篇论文的总体看法。

## 为什么还要改造 Dijkstra

如果只看运行时间，Section 7 已经完成了主要目标。但在 comparison model 中，论文还希望比较次数也达到 universal optimality。问题在于，Dijkstra 即使用了 working-set heap，仍然可能为每个顶点付出一些不必要的比较。这个额外的 $n$ 在一般情况下可能不明显，但当 $\log D$ 很小时就会变得重要。

Section 8 和 Section 9 做的事情不是重新发明最短路算法，而是在 Dijkstra 的框架上处理一些“顺序几乎已经被图结构决定”的顶点，避免为它们付出多余比较。

## bottleneck 的作用

这两节的共同概念是 bottleneck。论文先定义顶点的 level，大致表示从源点到该点的路径上最少顶点数。如果某一层只有一个顶点，那么这个顶点就是 bottleneck。

bottleneck 可以理解成所有更高层路径都必须经过的关口。因为这一层只有它一个点，所以到后面层的任何路径都绕不开它。这样一来，很多 bottleneck 的相对顺序其实已经由图结构确定，不一定需要像普通顶点那样放进堆里比较。

论文还证明，如果 bottleneck 不多，那么 $\log D$ 本身就不会太小，原来的比较上界已经可以接受；需要额外处理的是图里 bottleneck 很多的情况。因此，Section 8 和 Section 9 都围绕“如何单独处理 bottleneck”展开。

## Dijkstra with lookahead

Section 8 给出的第一个方法是 Dijkstra with lookahead。它先用 BFS 找出 bottleneck，然后运行类似 Dijkstra 的过程，但不把 bottleneck 插入堆。对于这些 bottleneck，算法会提前计算它们的真实距离，并在合适的时候把它们加入最终的 distance order。

这个方法的直觉比较清楚：既然 bottleneck 的位置在很大程度上由图结构决定，就不需要让它们参与普通堆操作和不必要比较。算法需要额外维护一个 bottleneck 数组，并在堆中最小顶点和当前 bottleneck 距离之间做合并式处理。

最后得到的结果是，Dijkstra with lookahead 可以在 $O(m+\log D)$ 时间内完成，并且比较次数达到 $O(F-n+1+\log D)$。这就和 Section 6 的两个下界都匹配了。

## recursive Dijkstra

Section 9 又给了另一种做法：recursive Dijkstra。它同样利用 bottleneck，但处理方式不同。算法从源点开始运行 Dijkstra，当遇到下一个 bottleneck 时，暂停当前运行，从这个 bottleneck 开始递归地开一个新的 Dijkstra 过程。等更高层的递归处理完，再回到较低层继续。

这个算法的好处是可以按 bottleneck 把图分成一段一段来处理。因为 bottleneck 控制了通往更高层的路径，所以递归地优先处理这些关键点是合理的。不过它不一定按照最终距离顺序扫描所有顶点，因此还需要一个支持局部插入的结构来维护输出顺序，论文中使用的是 finger search tree。

和 lookahead 版本一样，recursive Dijkstra 最后也能达到时间和比较次数的 universal optimality。两个算法给出同一个目标的不同实现方式，这一点对最终 review 也有价值：它说明论文的核心并不是某个偶然技巧，而是 bottleneck 结构确实抓住了比较次数差距的来源。

## 对整篇论文的总体看法

从整体看，这篇论文最值得强调的地方在于，它没有简单追求一个新的 $n,m$ 最坏情况复杂度，而是重新提出了一个更细的问题：对一张固定的图，Dijkstra 是否已经做到了它能做到的最好？论文给出的回答是，只要堆能够适应操作序列的局部性，并且对 bottleneck 做额外处理，Dijkstra 确实可以达到这种更强的最优性。

最终 review 可以重点突出三点贡献：

1. 把 universal optimality 引入到顺序图算法和 Dijkstra 分析中；
2. 发现 working-set heap 与 distance order 数量之间的联系；
3. 构造支持 decrease-key 的 working-set heap，并用 lookahead / recursive Dijkstra 补齐比较次数最优。

同时也需要写清楚它的限制。首先，论文的紧致结论主要针对 distance order problem，而不是所有形式的单源最短路问题。其次，Section 10 的堆结构比较复杂，实际实现价值可能不如理论意义明显。最后，证明中使用了不少组合和数据结构工具，如果 review 直接照论文顺序写，读者可能会觉得门槛较高，需要先解释直觉再写形式化证明。
