# AI 使用记录

本文件用于记录 AI 工具参与过的实质性阅读、翻译和整理环节。AI 输出只作为理解辅助，关键定义和结论以论文原文为准。

## 记录模板

### 日期：YYYY-MM-DD

- 使用工具：
- 提问内容：
- AI 输出摘要：

### 日期：2026-07-07

- 使用工具：ChatGPT
- 提问内容：帮我翻译并解释论文 Abstract、Introduction 和 Roadmap。
- AI 输出摘要：辅助理解 universal optimality、working-set bound、distance order problem，以及论文为什么重新分析 Dijkstra。

### 日期：2026-07-08

- 使用工具：Codex
- 提问内容：帮我梳理第 3 节中的关键定义。
- AI 输出摘要：整理了 distance order、true distance order、forward arc、dominator、useless arc、level 和 bottleneck 的区别与联系。

### 日期：2026-07-10

- 使用工具：ChatGPT
- 提问内容：帮我解释第 6 节 lower bounds 的证明思路。
- AI 输出摘要：概括了 `Ω(m)` 时间下界、`Ω(log D)` 决策树下界，以及 `Ω(F - n + 1)` 前向弧比较下界的直觉。

### 日期：2026-07-11

- 使用工具：Codex
- 提问内容：帮我理解第 7 节 working-set heap 为什么能得到 `O(m + log D)`。
- AI 输出摘要：解释了 Lemma 7.3 中 working-set size、区间 DAG、拓扑序数量和 `log D` 之间的关系。

### 日期：2026-07-12

- 使用工具：ChatGPT
- 提问内容：帮我梳理 Dijkstra with lookahead 的算法流程。
- AI 输出摘要：总结了 bottleneck 的 marked / unmarked 分类、数组 `B` 的作用，以及 Case 1、Subcase 2a、Subcase 2b 的处理方式。

### 日期：2026-07-14

- 使用工具：Codex
- 提问内容：帮我整理第 10 节 working-set heap 构造。
- AI 输出摘要：概括了 outer heap、inner heap、suffix minimum、bit vector 和 disjoint set union 在 heap 实现中的作用。

### 日期：2026-07-22

- 使用工具：Codex
- 提问内容：审查section 04，给出修改意见。
- AI 输出摘要：

针对 Section 4“主要技术路线”部分，AI 建议保留其路线图定位，避免提前展开 Section 6 中的证明细节。正文适合按论文主结论的闭合顺序组织：先说明 time model 和 comparison model 中需要匹配的下界，再说明普通 Dijkstra 通过 working-set heap 匹配时间下界，随后解释 comparison model 中剩余的差距为什么需要 bottleneck 技术，最后补充第 10 节 working-set heap 构造如何使前文的数据结构假设落地。

AI 建议在 working-set 部分突出 \(\sum_v \log W(v)=\mathrm{O}(\log D)\) 这条主线。这里不必在本节完整证明 interval DAG 引理，只需要说明顶点在堆中的生命周期区间如何把局部删除代价和固定图的 distance order 数量联系起来。

对于 bottleneck 部分，AI 建议同时提到 Dijkstra with lookahead 和 recursive Dijkstra，但只说明二者共同目标都是减少由拓扑强制顺序导致的冗余比较。Lookahead 通过数组单独推进 bottleneck，recursive Dijkstra 通过递归边界和 finger search tree 维护最终顺序；具体算法细节可以留到后文展开。

最后，AI 建议把第 10 节作为技术路线的收束点：如果没有支持 \texttt{decrease-\allowbreak{}key} 的 working-set heap，普通 Dijkstra 的时间上界仍只是条件性结论；作者给出 outer heap、inner heaps、union-find 和 suffix-minimum bit vector 的组合，才使主定理完整闭合。
