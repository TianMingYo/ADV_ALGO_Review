# recursive Dijkstra 笔记

## 阅读范围

本篇记录第 9 节 recursive Dijkstra。它是论文给出的另一个扩展版本，也用于把比较次数降到与下界匹配的量级。

## 基本思路

recursive Dijkstra 仍然利用 bottleneck 的支配性质。bottleneck 按 level 有天然顺序，低层 bottleneck 支配更高层顶点。

算法先从源点 `s` 开始运行 Dijkstra。当当前 run 即将扫描下一个 bottleneck 时，暂停当前 run，并从这个 bottleneck 开启一个新的 Dijkstra run。这个过程一直递归到最高层 bottleneck。

之后，算法再从高层往低层恢复并完成各个 Dijkstra run。这样可以优先确定 bottleneck 的真实距离，再补齐非 bottleneck 的距离和最终顺序。

## 多个 heap

recursive Dijkstra 会为不同 bottleneck 关联不同的 heap。每个 run 有自己的 heap，但算法不需要 meld 操作。

如果某次扫描发现一条弧可以改善其他 heap 中顶点的当前距离，就对对应 heap 中的顶点执行 `decrease-key`。论文证明这种跨 heap 的更新仍然保持正确性。

## 维护输出顺序

recursive Dijkstra 需要额外维护最终的 distance order 列表 `L`。它先把所有 bottleneck 按 level 放入 `L`。

当非 bottleneck 被扫描后，算法从它的父节点位置附近开始搜索，把该顶点插入到 `L` 中合适的位置。为了让这种局部插入足够快，论文使用 homogeneous finger search tree。

## 正确性直觉

正确性证明分两部分：

- 先证明每个顶点被扫描时，当前距离等于真实距离。
- 再证明最终列表 `L` 是一棵搜索树的拓扑序，因此根据 Lemma 3.1，它是 distance order。

bottleneck 的支配关系保证递归启动的顺序不会漏掉必要路径；父指针和局部插入保证最终顺序仍然尊重最短路树结构。

## 效率结论

Theorem 9.5 说明 recursive Dijkstra 在时间和比较次数上都达到常数因子内最优。

它和 Dijkstra with lookahead 的目标相同，但实现方式不同：lookahead 用数组 `B` 直接提前处理 bottleneck，而 recursive Dijkstra 用多个局部 Dijkstra run 和 finger search tree 维护最终顺序。
