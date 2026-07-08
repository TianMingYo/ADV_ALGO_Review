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
