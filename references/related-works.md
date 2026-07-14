# Related Works 整理

## 1. 文献整理

| 类别 | 文献 | 本地源文件 | 和主论文的关系 | 参考价值                                                                  |
| --- | --- | --- | --- |-----------------------------------------------------------------------|
| Dijkstra 基础 | Edsger W. Dijkstra, 1959, *A note on two problems in connexion with graphs* | `references/papers/dijkstra-1959-note-two-problems.pdf` | 最短路算法的经典来源。主论文不是重新提出 Dijkstra，而是重新分析它在实例结构下的最优性。 | 写“本文把经典 Dijkstra 放到 universal optimality 框架下重新理解”。                    |
| 堆结构基础 | Michael L. Fredman and Robert E. Tarjan, 1987, *Fibonacci heaps and their uses in improved network optimization algorithms* | `references/papers/fredman-tarjan-1987-fibonacci-heaps.pdf` | Fibonacci heap 是支持高效 `decrease-key` 的经典优先队列，也是最短路算法复杂度分析里的重要背景。 | 写“高效 heap 一直是 Dijkstra 复杂度分析的核心工具”。                                   |
| 堆结构基础 | Michael L. Fredman, Robert Sedgewick, Daniel D. Sleator, and Robert E. Tarjan, 1986, *The Pairing Heap: A New Form of Self-Adjusting Heap* | `references/papers/fredman-sedgewick-sleator-tarjan-1986-pairing-heap.pdf` | pairing heap 代表 self-adjusting heap 思路，和本文 working-set / 自适应分析的气质接近。 | 可作为“priority queue 结构不断追求更自适应”的背景。                                    |
| 堆结构基础 | Bernhard Haeupler, Siddhartha Sen, and Robert E. Tarjan, 2011, *Rank-Pairing Heaps* | `references/papers/haeupler-sen-tarjan-2011-rank-pairing-heaps.pdf` | 主论文第 10 节提到 inner heap 可以用 rank-pairing heap 这类 fast heap。 | 写第 10 节数据结构实现时作为 fast heap 的例子。                                       |
| 堆结构基础 | Thomas D. Hansen, Haim Kaplan, Robert E. Tarjan, and Uri Zwick, 2017, *Hollow Heaps* | `references/papers/hansen-kaplan-tarjan-zwick-2017-hollow-heaps.pdf` | hollow heap 也是第 10 节中可用的 fast heap 例子。 | 和 Fibonacci heap、rank-pairing heap 一起说明本文构造不是依赖单一 heap。               |
| Working-set / finger 性质 | Amr Elmasry, Arash Farzan, and John Iacono, 2012, *A priority queue with the time-finger property* | `references/papers/elmasry-farzan-iacono-2012-time-finger-priority-queue.pdf` | 和 working-set priority queue 同属“按访问/时间局部性自适应”的优先队列研究。 | 写“本文的 heap 继承了自适应优先队列的思路，但还必须支持 Dijkstra 需要的 `decrease-key`”。         |
| Finger search | Gerth Stølting Brodal, Rolf Fagerberg, and Riko Jacob, 2005, *Finger Search Trees* | `references/papers/brodal-fagerberg-jacob-2005-finger-search-trees.pdf` | 第 9 节 recursive Dijkstra 用到 finger search tree 相关思想。 | 用来补充解释第 9 节为什么需要 finger search。                                       |
| Beyond-worst-case / partial information | Bernhard Haeupler, Richard Hladík, John Iacono, Václav Rozhoň, Robert E. Tarjan, and Jakub Tětek, 2025, *Fast and Simple Sorting Using Partial Information* | `references/papers/haeupler-hladik-iacono-rozhon-tarjan-tetek-2025-fast-simple-sorting-partial-information.pdf` | 和主论文共享部分作者，也同样把算法复杂度和实例中的“已有信息/结构”联系起来。 | 写 related work 时说明主论文属于 beyond-worst-case / partial-information 分析脉络。 |
| Universal optimality | Bernhard Haeupler, David Wajc, and Goran Zuzic, 2021, *Universally-Optimal Distributed Algorithms for Known Topologies* | `references/papers/haeupler-wajc-zuzic-2021-universally-optimal-known-topologies.pdf` | universal optimality 近年在分布式算法中也有发展。主论文把类似思想推进到 Dijkstra / SSSP 相关问题。 | 用来说明 universal optimality 不是本文孤立提出的概念，而是一条研究线。                        |
| SSSP 近期背景 | Ran Duan, Jiayi Mao, Xiao Mao, Xinkai Shu, and Longhui Yin, 2025, *Breaking the Sorting Barrier for Directed Single-Source Shortest Paths* | `references/papers/duan-mao-mao-shu-yin-2025-directed-sssp.pdf` | 和 directed SSSP 的最新复杂度进展相关，但它关注的是 worst-case 算法突破，和本文的 universal optimality 角度不同。 | 可在 related work 里对比：一类工作追求 worst-case 更快，本文追求实例级最优。                   |

## 2. 源文件下载来源

| 本地文件 | 来源 |
| --- | --- |
| `dijkstra-1959-note-two-problems.pdf` | <https://jmvidal.cse.sc.edu/library/dijkstra59a.pdf> |
| `fredman-tarjan-1987-fibonacci-heaps.pdf` | <https://ftp.eecs.umich.edu/~pettie/matching/Fredman-Tarjan-Fibonacci-Heaps.pdf> |
| `fredman-sedgewick-sleator-tarjan-1986-pairing-heap.pdf` | <https://www.cs.cmu.edu/~sleator/papers/pairing-heaps.pdf> |
| `haeupler-sen-tarjan-2011-rank-pairing-heaps.pdf` | <https://sidsen.azurewebsites.net/papers/rp-heaps-journal.pdf> |
| `hansen-kaplan-tarjan-zwick-2017-hollow-heaps.pdf` | <https://arxiv.org/pdf/1510.06535> |
| `elmasry-farzan-iacono-2012-time-finger-priority-queue.pdf` | <https://arxiv.org/pdf/1009.5538> |
| `brodal-fagerberg-jacob-2005-finger-search-trees.pdf` | <https://cs.au.dk/~gerth/papers/finger05.pdf> |
| `haeupler-hladik-iacono-rozhon-tarjan-tetek-2025-fast-simple-sorting-partial-information.pdf` | <https://arxiv.org/pdf/2404.04552> |
| `haeupler-wajc-zuzic-2021-universally-optimal-known-topologies.pdf` | <https://arxiv.org/pdf/2104.03932> |
| `duan-mao-mao-shu-yin-2025-directed-sssp.pdf` | <https://arxiv.org/pdf/2504.17033> |