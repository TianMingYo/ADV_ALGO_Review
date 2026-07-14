# 第 11 节阅读笔记

## 1. 本节目标

第 11 节是全文的总结，主要在说：

* 本文已经证明了什么；
* universal optimality 这个思路还能往哪里推广；
* 还有什么问题没有解决。

## 2. 总结

作者最后总结了三点：

第一：

* 如果 Dijkstra 使用足够高效的 heap，那么它在 running time 上是 universally optimal 的。

这里的 heap 指的就是前面需要的 working-set heap。

第二：

* Dijkstra 的两个扩展版本，在 running time 和 comparisons 上都是 universally optimal 的。

这里对应的是：

* Dijkstra with lookahead；
* recursive Dijkstra。

应该是说普通 Dijkstra 在时间模型下已经很好，但在比较次数模型下还需要进一步处理 bottleneck，所以才有后面两个扩展算法。

第三：

* 论文构造了需要的 heap。

## 3. open problem

作者觉得 universal optimality 这个概念不一定只适用于本文的问题，还可能用于别的问题。

举的例子是 single-source single-target version of the distance order problem。

这个问题和本文原问题的区别是：

* 本文原问题是从源点 `s` 出发，输出完整的 distance order；
* 这里还给定一个目标点 `t`；
* 算法只需要输出一个包含 `s` 和 `t` 的前缀，不需要输出完整顺序。

如果用 Dijkstra 来做，很自然的办法是当 t 从 heap 中被 delete-min 时，直接停止

因为这时从 `s` 到 `t` 的距离已经确定了。

要让这个提前停止版本也 universally optimal，需要更强的 working-set heap，原因如下：

* 普通 working-set bound 会统计插入过 heap 的元素，但是 single-source single-target 的算法可能提前停止；
* 有些元素被插入过 heap，但不会被删除，如果这些元素仍然被计入 working set，分析不够精确。

所以需要一种更强的 bound，即不要统计那些插入过但最终没有被删除的元素，同时这个 heap 还要支持：

```text
decrease-key: O(1) amortized
```

## 4.当前理解

这一节对最终 review 很有用，因为它能直接支撑两个部分。

第一，可以用来写本文贡献：

* 证明 Dijkstra 在合适 heap 下具有 running-time universal optimality；
* 通过两个扩展算法进一步处理 comparison complexity；
* 构造 supporting data structure，让算法分析闭合。

第二，可以用来写局限和未来方向：

* 本文解决的是 single-source distance order；
* 对于 single-source single-target 的提前停止版本，还没有完整解决；
* 关键困难转移到了更强的 working-set heap 上。

最后review的过程大概可以如下：

```text
定义问题和实例难度
        ↓
证明下界
        ↓
设计或改造 Dijkstra 来匹配下界
        ↓
补上需要的数据结构
        ↓
指出下一步可能推广的问题
```

这也说明最终 review 不能只写“这篇论文改进了 Dijkstra”，而应该写成：

```text
这篇论文把 Dijkstra 放进 beyond-worst-case / universal optimality 的框架里重新分析，
并说明在合适数据结构支持下，经典算法可以达到按实例结构刻画的最优性。
```

## 5. 还需要继续确认的问题

* single-source single-target 为什么比完整 distance order 更难证明 universal optimality？
* 不统计“插入但未删除”的元素，为什么会让 working-set heap 更难设计？
* 最终 review 里要不要把这个 open problem 作为局限性来写？
