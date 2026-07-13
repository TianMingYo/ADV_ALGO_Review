# 过程日志

## 2026-07-07

- 阅读论文 *Universal Optimality of Dijkstra via Beyond-Worst-Case Heaps* 的 Abstract、Introduction 和 Roadmap。
- 确认第 1 次阅读记录重点：选题原因、论文主问题、整体证明路线，以及 universal optimality、Dijkstra、working-set heap 之间的初步关系。
- 整理论文并不是提出全新最短路算法，而是讨论 Dijkstra 在 distance order problem 上的 universal optimality。
- 记录后续需要关注的问题：universal optimality 和 instance optimality 的区别，$D$、$F$ 两个参数进入上下界的原因，以及 working-set size 与 distance order 数量之间的关系。

## 2026-07-08

- 阅读论文 Related Work，并补充 Dijkstra、Fibonacci heap、最短路算法和 working-set bound 的背景。
- 梳理 instance optimality 与 universal optimality 的区别，确认本文固定的是图结构而不是完整输入。
- 记录 Fibonacci heap 在 Dijkstra 分析中的作用和局限，重点是 `delete-min` 仍按堆整体规模分析。
- 明确后续需要继续区分 distance order problem 与普通 SSSP，并理解支持 `decrease-key` 的 working-set heap 为什么需要单独构造。

## 2026-07-11

- 阅读论文 Section 3–5，梳理 distance order、true distance order、$D$、$F$、复杂度模型和 universal optimality 的正式定义。
- 确认 distance order 是由图结构允许的顶点顺序，而 true distance order 是给定边权后的真实距离顺序。
- 记录 Dijkstra 扫描顺序与 true distance order 的关系，以及为什么 distance order problem 可以被单独拿出来分析。
- 区分 comparison model 和 time model，为后续 Section 6 的下界证明做准备。

## 2026-07-12

- 阅读论文 Section 6，整理 time model 和 comparison model 下的下界证明结构。
- 梳理 $\Omega(m+\log D)$ 时间下界的来源：算法既需要访问图中的边信息，也需要区分图结构允许的 distance orders。
- 记录 comparison model 中 $\Omega(F-n+1+\log D)$ 下界的直觉，重点关注额外 forward arcs 带来的比较需求。
- 理解 Lemma 6.4 中通过扰动边权说明比较次数不足的反例思路。

## 2026-07-13

- 阅读论文 Section 7，并结合 Section 10 理解 working-set heap 的高层设计。
- 梳理 Dijkstra 使用 working-set heap 后达到 $O(m+\log D)$ 时间上界的证明主线。
- 记录 working-set 区间、interval DAG、search tree 与 distance order 数量 $D$ 之间的关系。
- 整理分层内层堆、union-find 和 bit vector 在 working-set heap 中各自承担的作用。
