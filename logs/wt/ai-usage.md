# AI 使用记录

本文件用于记录本人在论文阅读、笔记整理和 review 写作过程中使用 AI 工具的情况。

## 2026-07-07

- 使用工具：GPT 网页版
- 对应文件：`notes/wt/note1.md`
- 使用内容：使用 AI 辅助根据大作业要求和论文目录确认阅读记录的安排，并辅助梳理 universal optimality、Dijkstra 和 working-set heap 之间的关系。
- 核验方式：AI 的内容只作为理解辅助，本次记录中的论文结论主要根据 Abstract、Introduction 和 Roadmap 重新核对后写入。

## 2026-07-08

- 使用工具：GPT 网页版
- 对应文件：`notes/wt/note2.md`
- 使用内容：使用 AI 辅助梳理 Related Work 中几条背景线索，并检查本次笔记是否会和第一次阅读笔记冲突。AI 的作用主要是帮助组织阅读内容和提醒需要区分 distance order problem 与一般 SSSP。
- 核验方式：涉及论文结论和相关工作的地方，仍然回到原论文和检索到的论文页面进行核对。

## 2026-07-11

- 使用工具：GPT 网页版
- 对应文件：`notes/wt/note3.md`
- 使用内容：使用 AI 辅助梳理 Section 3–5 中几个定义之间的关系，尤其是 distance order、true distance order、$D$、$F$ 和 universal optimality 的联系。AI 主要用于检查概念表述是否前后矛盾。
- 核验方式：具体定义和结论仍然以论文原文为准。

## 2026-07-12

- 使用工具：GPT 网页版
- 对应文件：`notes/wt/note4.md`
- 使用内容：使用 AI 辅助梳理 Section 6 的证明结构，重点是区分 $\Omega(m)$、$\Omega(\log D)$ 和 $\Omega(F-n+1)$ 三个来源。
- 核验方式：AI 帮助将 Lemma 6.4 的证明思路整理成更直观的说法，但具体结论和符号仍然以论文原文为准。

## 2026-07-13

- 使用工具：GPT 网页版
- 对应文件：`notes/wt/note5.md`
- 使用内容：使用 AI 辅助整理 Section 7 的证明路线，尤其是 working-set 区间、interval DAG、search tree 和 $D$ 之间的关系。AI 还用于将 Section 10 的堆结构概括为高层设计思路。
- 核验方式：具体定理表述和复杂度界限仍然以论文原文为准。

## 2026-07-14

- 使用工具：GPT 网页版
- 对应文件：`notes/wt/note6.md`
- 使用内容：使用 AI 辅助梳理 Section 8 和 Section 9 的共同主线，尤其是 bottleneck 为什么能减少比较次数，以及 Dijkstra with lookahead 和 recursive Dijkstra 的区别。AI 还用于整理最终 review 可能采用的结构。
- 核验方式：相关算法结论和复杂度表述仍然以论文原文为准。

## 2026-07-20

- 使用工具：Codex
- 提问内容：根据论文内容和前期阅读笔记，辅助撰写 review 的 Section 1“论文解决的问题”。
- AI 输出摘要：

AI 首先协助确定了本节的切入方式。初稿没有直接从 universal optimality 的形式化定义开始，而是先回到常见的 Dijkstra 算法：它在运行过程中会同时给出最短距离、最短路树和顶点扫描顺序。Section 1 将第三个输出单独拿出来，由此引出论文实际研究的 distance order problem，避免一开始就堆叠符号和复杂度结论。

在 distance order 和 true distance order 的表述上，AI 帮助区分了“图结构允许出现的顺序”和“给定边权后算法应输出的顺序”。正文进一步说明 true distance order 除了要求真实距离非递减，还要求每个非源点具有前向入弧。这个条件用于处理零长度边导致的 ties；而在下界构造中，可以选择距离互异的赋权，不必把 ties 混入主要论证。

随后，本节把分析对象从所有同规模图收缩到固定拓扑。AI 协助整理了参数 $D$ 和 $F$ 的引入顺序：$D$ 表示固定图可能产生的 distance order 数量，$\log D$ 对应区分这些顺序所需的信息量；$F$ 是某个 distance order 中 forward arcs 数量的最大值，扣除每个非源点至少需要的一条基本前向入弧后，得到 $F-n+1$。在此基础上，正文给出 time model 中 $\mathrm{O}(m+\log D)$ 和 comparison model 中 $\mathrm{O}(F-n+1+\log D)$ 两个目标复杂度。

在 universal optimality 的定义部分，AI 帮助把它与一般 worst-case optimality 和 instance optimality 分开。正文固定图 $G$，用 $\mathrm{OPT}(G)$ 表示所有正确算法在该图全部合法边权赋值上的最小最坏代价；一个统一算法如果对每张固定图都达到 $\mathrm{O}(\mathrm{OPT}(G))$，才称为 universally optimal。这里不是针对每个带权实例临时选择不同算法，而是让同一个算法适应不同固定拓扑的难度。

最后，AI 协助补充了 time model 和 comparison model 的计费差异。Time model 会计算邻接表访问、边扫描、RAM 操作和比较，因此 $m$ 必须出现在复杂度中；comparison model 把图结构视为已知，只计算边长表达式、路径长度或 priority queue key 之间的比较。这样可以避免把 comparison bound 中的 $F-n+1$ 误解为算法只需要查看这些边。完成的初稿见 `drafts/review/sections/01_problem.tex`。
