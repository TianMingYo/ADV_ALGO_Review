# 问题清单

记录阅读过程中暂时不懂、需要后续细读或需要核验的问题。

| 日期 | 问题 | 来源位置 | 当前状态 | 核验方式 |
| - | - | - | - | - |
| 2026-07-07 | distance order 的正式定义为什么可以只靠前向入弧刻画？ | Roadmap / Section 3 预告 | 已解决 | 对照 Lemma 3.1 |
| 2026-07-07 | `D` 和 `F` 两个参数分别如何进入下界和上界？ | Roadmap | 已解决：`D` 控制顺序自由度，`F - n + 1` 控制额外前向弧比较，二者共同出现在上下界中 | 对照 Section 6-8 |
| 2026-07-07 | working-set heap 的 `delete-min` 代价为什么能被 `log D` 控制？ | Abstract / Roadmap | 已解决：把插入到删除的时间段看作区间，用 interval DAG 的拓扑序数量联系到 distance order 数量 | 对照 Lemma 7.3 |
| 2026-07-08 | distance order 和 true distance order 是否相同？ | Section 3 | 已解决：distance order 是结构上可能出现的顺序，true distance order 是给定边权下实际按真实距离排列且满足前向入弧条件的顺序 | 对照 Section 3 定义 |
| 2026-07-09 | 为什么 `F - n + 1` 会出现在 Dijkstra 的比较次数里？ | Lemma 4.2 | 已解决：`n - 1` 条前向弧负责首次发现非源点，剩余前向弧才可能触发比较和 `decrease-key` | 对照 Lemma 4.2 |
| 2026-07-10 | `log D` 下界为什么像排序下界？ | Lemma 6.2 | 已解决：不同 distance order 可以对应决策树中的不同叶子，算法必须区分至少 `D` 种可能 | 对照 Lemma 6.2 |
| 2026-07-11 | working-set size 如何联系到 `D`？ | Lemma 7.3 | 已解决：顶点堆生命周期对应区间，区间 DAG 的拓扑序数量被原图 distance order 数量控制 | 对照 Lemma 7.1、Lemma 7.3 |
| 2026-07-12 | bottleneck 为什么可以提前处理？ | Section 8 | 已解决：bottleneck 支配更高层顶点，连续 unmarked bottleneck 的距离可以沿层级推出 | 对照 Lemma 8.2、Lemma 8.3、Theorem 8.4 |
