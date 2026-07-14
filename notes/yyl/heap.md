# working-set heap 构造笔记

## 阅读范围

本篇记录第 10-11 节，重点是论文如何构造真正满足 working-set bound、且支持 `decrease-key` 的 heap。

## 构造目标

论文需要一个 heap 支持：

- `make-heap`
- `find-min`
- `insert`
- `decrease-key`
- `delete-min`

其中 `insert` 和 `decrease-key` 要 `O(1)` 摊还，删除元素 `x` 的 `delete-min` 要 `O(log W(x))` 摊还。这里 `W(x)` 是 `x` 的 working-set size。

## outer heap 与 inner heap

论文的构造是 black-box 的：从任意具有 Fibonacci heap 级别效率的 fast heap 出发，把它们作为 inner heap，再用 outer heap 组织这些 inner heap。

outer heap 维护一串 inner heap：

`H_1, H_2, ..., H_k`

较小编号的 inner heap 存放较晚插入的元素。如果 `i < j`，则 `H_i` 中元素都晚于 `H_j` 中元素插入。这样删除较老元素时，它前面已经积累了足够多后插入元素，可以用 working-set size 支付删除成本。

## 插入时的合并规则

插入新元素时，先建立一个只含新元素的 `H_0`。然后寻找最小的 `j`，使 `H_j` 和 `H_{j+1}` 合并后仍然足够小。

如果找到了，就把二者 meld 成新的 `H_{j+1}`，并重新编号较小的 heap；如果找不到，就只重新编号。

“足够小”的阈值是双指数形式，所以 inner heap 数量最多是 `1 + log log n`。这使得维护外层结构的代价很低。

## 维护 item 所在 inner heap

`decrease-key` 必须知道元素在哪个 inner heap 中。论文把这个问题归约为 disjoint set union。每个 inner heap 对应一个集合，inner heap 合并时集合也合并。

在 Dijkstra 的使用场景中，每个插入元素最终都会被删除，因此可以用较简单的 compressed tree 加 linking by index 获得需要的摊还效率。

## 维护全局最小值

`find-min` 和 `delete-min` 需要快速找到包含全局最小 key 的 inner heap。论文维护 suffix minimum：如果 `H_i` 非空且其最小 key 小于所有更大编号非空 inner heap 的最小 key，则 `H_i` 是 suffix minimum。

这些 suffix minimum 用一个很短的 bit vector 表示。由于 inner heap 数量只有 `O(log log n)`，bit vector 可以放在一个机器字中，用常数时间支持 next / prev 查询。

## 小结

Theorem 10.6 表明，只要能常数摊还地找到元素所在 inner heap 和包含最小元素的 inner heap，outer heap 就满足 working-set bound。

第 10 节的意义是：论文不仅假设存在合适的 heap，还给出了可以支持 `decrease-key` 的实现框架。第 11 节则指出，类似的 universal optimality 思路还可能用于其他最短路变体。
