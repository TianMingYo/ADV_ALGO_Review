# 第 9 节阅读笔记

## 1. 本节目标

第 8 节已经给出 Dijkstra with lookahead，可以同时匹配时间下界和比较下界。

第 9 节又给出另一个版本：

* recursive Dijkstra。

它也是为了达到：

* 时间最优；
* 比较次数最优。

区别在于：

* 第 8 节是用数组 `B` 单独处理 bottleneck。
* 第 9 节是遇到 bottleneck 时，直接启动一个新的 Dijkstra 递归运行。

## 2. 核心直觉

bottleneck 有性质：低层 bottleneck 支配所有更高层顶点。

所以第 9 节的想法是：

* 先优先算出所有 bottleneck 的真实距离；
* 遇到下一个 bottleneck 时，暂停当前 Dijkstra；
* 从这个 bottleneck 开启一个新的 Dijkstra；
* 等最高层 bottleneck 的运行完成后，再从高到低恢复之前暂停的运行。

大概就是：

* 递归版其实是在把整张图按 bottleneck 切成一段一段。
* 每个 bottleneck 像是一个新的局部源点。
* 先确定这些关键源点，再补完中间的非 bottleneck。

## 3. Recursive Dijkstra 怎么运行

初始化：

* 找出所有 bottleneck。
* 设置 `d(s) = 0`，其他点距离为无穷。
* 初始化列表 `L`，先把 bottleneck 按层数递增放进去。
* 调用 `Dijkstra(s)`。

递归过程 `Dijkstra(u)`：

* 给当前源点 `u` 建一个堆 `H(u)`（每个 bottleneck 对应一个堆来运行）。
* 扫描 `u`，不断从 `H(u)` 中 `delete-min`。
* 如果弹出的点是 bottleneck，就递归调用 `Dijkstra(v)`。
* 如果弹出的点不是 bottleneck，就正常扫描它

## 4. 列表 `L` 怎么维护

bottleneck 一开始已经按层数放进 `L`。

非 bottleneck 被扫描后，要插入到 `L` 的合适位置，从父亲 `p(v)` 附近开始找：

* 找到父亲后面第一个距离不小于 `v` 的顶点；
* 把 `v` 插到它前面；
* 如果找不到，就放到最后。

为了让这个局部搜索高效，论文用 finger search tree，另开一个note单独学习记录。

## 5. 正确性

Theorem 9.1 证明 recursive Dijkstra 正确。

要证明两件事：

* 每个顶点被扫描时，当前距离是真实距离。
* 最后得到的 `L` 是 distance order。

距离正确性的直觉：

* 每个 bottleneck 被作为新源点启动时，它的真实距离已经确定。
* 每个局部 Dijkstra 运行都像普通 Dijkstra，只是起点距离不是 0，而是这个 bottleneck 的真实距离。
* 高层运行可能会更新低层堆中的点，但这些更新正是保持全局最短路正确所需的。

`L` 是 distance order 的直觉：

* 非 bottleneck 插入时放在父亲之后。
* bottleneck 按层数排列，而低层 bottleneck 支配高层 bottleneck。
* 所以每个点都在其最短路树父亲之后。
* 因此 `L` 是搜索树的拓扑序，再由 Lemma 3.1 得到它是 distance order。

## 6. 效率

第 9 节最后也要得到最优时间和比较次数。

主要代价分几类：

* 找 bottleneck：`O(m)`。
* heap insert：总时间 `O(n)`，比较次数由 `n - b = O(log D)` 控制。
* decrease-key：最多 `F - n + 1` 次。
* delete-min：Lemma 9.2 控制为 `O(log D)`。
* 插入列表 `L` 的搜索：Lemma 9.3 控制为 `O(log D)`。

Lemma 9.2：

* 把递归运行产生的搜索树按 marked bottleneck 切成若干子树。
* 每个子树内部可以套用第 7 节的 working-set 分析。
* 所有子树合起来仍然由 `log D` 控制。

Lemma 9.3：

* 每次向 `L` 插入一个非 bottleneck，可以看成一个区间问题。
* 用 Lemma 7.1 的区间 DAG 工具，证明所有 finger search 的总代价是 `O(log D)`。

最终 Theorem 9.5：

* recursive Dijkstra 的时间和比较次数都在常数因子内最优。

## 7. 和第 8 节的关系

第 8 节：

* 用 lookahead；
* 用数组 `B` 成段处理 bottleneck；
* 直接证明比较次数达到下界。

第 9 节：

* 用递归；
* 每遇到 bottleneck 就开启一个新的 Dijkstra 运行；
* 用 finger search tree 维护最终顺序。

两者的共同点：

* 都利用 bottleneck 的支配性质。
* 都是为了去掉普通 Dijkstra 比较次数中多出来的 `n` 项。
* 都能达到 universal optimality。

## 8. 当前理解

第 9 节可以理解成另一种“利用 bottleneck 分解图”的方式。

第 8 节更像是提前看一段 bottleneck，第 9 节更像是把 bottleneck 当作递归分界点。

我觉得第 9 节的难点不在 Dijkstra 本身，而在：

* 多个 Dijkstra 运行之间如何交错；
* 为什么这种交错仍然保证真实距离；
* 最后列表 `L` 的插入代价为什么能用 `log D` 控制。

## 9. 还需要继续确认的问题

* 多个堆之间的 `decrease-key` 具体如何定位顶点所在堆。
* Theorem 9.1 的第二阶段归纳证明还需要再细读。
* Lemma 9.3 中区间 DAG 和 finger search tree 的对应关系需要画图理解。
