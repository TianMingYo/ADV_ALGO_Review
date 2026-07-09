# 问题清单

记录阅读过程中暂时不懂、需要后续细读或需要核验的问题。

| 日期 | 问题 | 来源位置 | 当前状态 | 核验方式 |
| - | - | - | - | - |
| 2026-07-07 | distance order 的正式定义为什么可以只靠前向入弧刻画？ | Roadmap / Section 3 预告 | 已解决 | 对照 Lemma 3.1 |
| 2026-07-07 | `D` 和 `F` 两个参数分别如何进入下界和上界？ | Roadmap | 部分解决：已理解 `D` 和 `F` 的定义，具体下界仍待阅读 | 后续对照 Section 5-7 |
| 2026-07-07 | working-set heap 的 `delete-min` 代价为什么能被 `log D` 控制？ | Abstract / Roadmap | 待阅读 Section 7 | 后续对照 Lemma 7.3 |
| 2026-07-08 | distance order 和 true distance order 是否相同？ | Section 3 | 已解决：distance order 是结构上可能出现的顺序，true distance order 是给定边权下实际按真实距离排列且满足前向入弧条件的顺序 | 对照 Section 3 定义 |
| 2026-07-09 | 为什么 `F - n + 1` 会出现在 Dijkstra 的比较次数里？ | Lemma 4.2 | 已解决：`n - 1` 条前向弧负责首次发现非源点，剩余前向弧才可能触发比较和 `decrease-key` | 对照 Lemma 4.2 |
