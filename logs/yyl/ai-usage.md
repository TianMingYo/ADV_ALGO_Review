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

### 日期：2026-07-22

- 使用工具：Codex
- 提问内容：审查section 05，给出修改意见。
- AI 输出摘要：

针对 Section 5“技术直觉”部分，AI 建议将重点放在三个直觉层面，而不是写成定理证明。第一，working-set 分析的核心是把一次昂贵的 \texttt{delete-\allowbreak{}min} 和图中可产生的 distance order 数量联系起来：顶点在堆中停留越久，说明它与更多后插入顶点共同竞争，也更可能对应更多顺序自由度，因此其代价可以由 \(\log D\) 支付。

第二，AI 建议把 \(F\) 和 \(F-n+1\) 解释为 forward arcs 带来的额外比较压力。每个非源点至少需要一条前向入弧用于首次发现，因此 \(n-1\) 条前向弧属于基本可达性需求；真正额外的比较来自其他可能提供替代路径的前向弧。

第三，AI 建议突出 bottleneck 与 working-set 直觉的互补关系。Working-set 处理的是“等待越久，可能顺序越多”；bottleneck 处理的是“顺序已经由拓扑强制，就不应继续放入普通堆中比较”。Dijkstra with lookahead 和 recursive Dijkstra 虽然实现不同，但都利用 bottleneck 的支配关系来减少普通 Dijkstra 在 comparison model 中多出的比较。

最后，AI 建议用较简短的语言说明第 10 节 heap 构造的直观意义：outer heap 按插入时间分层，年轻元素删除便宜，老元素删除较贵但有更大的 working set 支付；union-find 和 suffix-minimum bit vector 则用于让这种记账方式能够在优先队列操作中实现。
