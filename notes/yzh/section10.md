# 第 10 节阅读笔记

## 1. 本节目标

前面第 7-9 节都依赖一个假设：

* 存在一个支持 `decrease-key` 的 heap；
* 除了 `delete-min`，其他操作都是 `O(1)` 摊还；
* 删除元素 `x` 的 `delete-min` 代价是 `O(log W(x))`。

第 10 节就是要真的构造这个 heap。

也就是说，第 10 节不是再分析 Dijkstra，而是补上前面所有算法需要的数据结构。

## 2. 为什么已有 heap 不够

已有一些 heap 在不支持 `decrease-key` 的情况下可以有 working-set bound，但 Dijkstra 需要很多 `decrease-key`。

所以论文需要一个这样的新数据结构：

* 要有 working-set bound；
* 还要支持 `decrease-key`；
* 不需要对外支持 `meld`。

## 3. outer heap 和 inner heap

论文构造的堆叫 outer heap，里面不是一个堆，而是一串 inner heaps：

```text
H_1, H_2, ..., H_k
```

每个 inner heap 是一个 fast heap，比如 Fibonacci heap、hollow heap、rank-pairing heap。

fast heap 的要求：

* `insert`、`decrease-key`、`meld` 等操作是 `O(1)` 摊还；
* `delete-min` 在大小为 `n` 的堆上是 `O(log n)`。

outer heap 的关键不变量：

* 小编号 inner heap 里的元素比大编号 inner heap 里的元素更晚插入。

也就是：

```text
H_1 里是比较新的元素
H_2 稍旧
H_3 更旧
...
```

## 4. 为什么这样能得到 working-set bound

如果一个元素在比较靠后的 `H_i` 中，说明它比较旧。

根据插入顺序不变量：

* 前面的 `H_1, ..., H_{i-1}` 里的元素都比它晚插入；
* 这些元素都会进入它的 working set。

所以：

* 元素越旧，它的 working set 越大；
* 删除旧元素时，即使所在 inner heap 比较大，`log W(x)` 也足够支付删除代价。

这是第 10 节最核心的直觉。

## 5. 为什么 inner heap 大小是loglog

论文维护大概这样的上界：

```text
|H_i| <= 2^(2^i)
```

也就是说：

```text
H_1 很小
H_2 可以大一些
H_3 更大
...
```

双指数增长的好处：

* inner heap 数量很少，最多 `1 + log log n` 个；
* 删除 `H_i` 中元素的代价大概是 `log |H_i|`，也就是 `O(2^i)`；
* 而 `H_i` 中元素的 working set 也已经足够大，`log W(x)` 可以覆盖这个代价。

## 6. 插入怎么做

插入新元素时：

* 先建一个临时的一元素堆 `H_0`；
* 找最小的 `j`，使得 `H_j` 和 `H_{j+1}` 合并后还不太大；
* 如果找到，就 meld 这两个 inner heaps；
* 然后把前面的堆重新编号；
* 如果找不到，就只做重新编号。

## 7. Theorem 10.6 的意思

核心内容大概是：只要能做到两件事是 `O(1)` 摊还：

* 找到某个元素所在的 inner heap；
* 找到包含全局最小元素的 inner heap；

那么 outer heap 就有 working-set bound。

所以第 10 节后半部分其实是在解决这两个查询问题。

## 8. 如何找到元素所在的 inner heap

因为 inner heaps 会 meld，所以元素所属的集合会不断合并。这正好是一个类似DSU的 union-find 问题：

* 每个 inner heap 对应一个集合；
* 每次 meld inner heaps，就 unite 两个集合；
* `decrease-key(x)` 时，用 find 找到 `x` 当前属于哪个集合，也就是哪个 inner heap。

论文还用了一个 linking by index 的规则：

* 合并 `H_j` 和 `H_{j+1}` 时，让 `H_{j+1}` 的根当新根。

这样可以把 find 的额外代价控制在 working-set bound 里。

## 9. 如何找到全局最小元素所在的 inner heap

这是 10.4 的内容。

作者定义 suffix minimum：

* 如果 `H_i` 非空，并且它的最小键比所有更大编号 inner heap 的最小键都小，那么 `H_i` 是 suffix minimum。

维护一个 bit vector `b`：

* `b_i = 1` 表示 `H_i` 是 suffix minimum；
* `b_i = 0` 表示不是。

全局最小元素一定在最小编号的 suffix minimum 里。

所以 `find-min` 只需要：

```text
找到第一个为 1 的 bit
```

也就是 `next(1)`。

因为 inner heap 数量只有 `O(log log n)`，bit vector 很短，可以放进一个机器字里。所以 `next`、`prev`、读写 bit 都可以看成 `O(1)`。

## 10. 维护 bit vector 的代价

`delete-min`、`insert`、`decrease-key` 都可能改变 suffix minimum。

Lemma 10.8 说明维护 bit vector 的总代价仍然在 working-set bound 内。

直觉：

* 如果更新涉及很靠后的 `H_j`，说明对应元素的 working set 已经很大，可以支付 `O(j)` 的更新代价。
* `decrease-key` 可能让一些 bit 从 1 变成 0，这些代价可以摊还到之前把它们从 0 变成 1 的操作上。

## 11. Heap rebuilding

outer heap 的空间和总插入次数有关。如果很多元素已经被删除，空间可能比当前堆大小大很多。标准办法是维护数据结构常用的 rebuild：

* 当前大小下降太多时重建；
* 或者自上次重建后插入太多时重建。

重建花线性时间，可以摊还掉，不影响复杂度。

## 12. 当前理解

第 10 节可以理解成给前面算法补上数据结构基础。

前面第 7-9 节一直在说：

```text
假设有一个满足 working-set bound 的 heap
```

第 10 节证明：

```text
这样的 heap 真的可以实现
```

它的主要结构是：

```text
outer heap
  = 按插入时间分层的一串 inner heaps
  + union-find 维护元素属于哪个 inner heap
  + bit vector 维护哪个 inner heap 有全局最小值
```

## 13. 还需要继续确认的问题

* 插入时选择最小 `j` 并 meld 的规则，还需要再画例子理解。
* Lemma 10.4 / Corollary 10.5 中 working-set size 的下界推导还可以再细读。
* bit vector 维护 suffix minimum 的过程比较细，最终 review 里可能只需要讲直觉。
