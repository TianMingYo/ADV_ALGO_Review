# 过程日志

## 2026-06-22

* 初始化 Git 仓库、检索论文。

## 2026-06-25

* 在 Codex 辅助下初读 Abstract、Related Work。

## 2026-06-26

* 在 Codex 辅助下初读 Concept 部分。
* 整理 Concept 部分的相关概念。

## 2026-06-29

* 在 Codex 辅助下初读 section 4~6，理解了理论基础。
* 整理了相关的 note 与等待细读的问题。

## 2026-07-06

* 在 Codex 辅助下初读 section 7，理解 Dijkstra 算法如何匹配 Section 6 给出的时间下界。
* 理解了 working-set heap 在分析中的作用：`insert` 和 `decrease-key` 比较便宜，关键是控制所有 `delete-min` 的总代价。
* 初步确认普通 Dijkstra 在 time model 下已经是 universally optimal。

## 2026-07-07
* 在 Codex 辅助下初读 section 8，理解 Dijkstra with lookahead 如何处理 bottleneck。
* 整理了 section 8 note，记录 marked / unmarked bottleneck、lookahead 的算法直觉和正确性思路。
* 初步确认 Dijkstra with lookahead 可以达到 `O(F - n + 1 + log D)` 比较次数。
* 整理项目结构，区分每个人阅读的进度。

## 2026-07-08
* 在 Codex 辅助下初读 section 9，理解 recursive dijkstra 解决的问题和对 bottleneck 的不同处理方法。
* 在 ChatGPT 辅助下学习文中的 finger search tree 数据结构，并理解为什么该章需要使用该数据结构。

## 2026-07-09
* 在 Codex 辅助下初读 section 10，理解在前文铺垫下我们需要一个怎样的 heap，然后如何实现这样的一个 heap。
* 在 ChatGPT 辅助下学习该堆的实现，梳理该数据结构的原理，考虑如何写进 review 里。

## 2026-07-12
* 在 Codex 辅助下初读 section 11，基本通读并理解了全文。
* 思考 open problem。

## 2026-07-13~2026-07-14
* 整理 related work 相关的 reference 文献。