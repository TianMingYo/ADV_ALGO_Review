# AI 使用记录

本文件用于记录 AI 工具参与过的所有实质性环节。每次使用 AI 后，请补充一条记录，并在后续 commit 中保留。

## 记录模板

### 日期：YYYY-MM-DD

- 使用工具：
- 提问内容：
- AI 输出摘要：

### 日期：2026-06-25

- 使用工具：Codex
- 提问内容：帮我翻译 abstract 和 related work。
- AI 输出摘要：见 `drafts/abstract-zh.tex` 和 `drafts/related-work-zh.tex`。

### 日期：2026-06-26

- 使用工具：Codex
- 提问内容：帮我翻译第三部分的概念。
- AI 输出摘要：见 `drafts/section3-zh.tex`。

### 日期：2026-06-29

- 使用工具：Codex
- 提问内容：帮我翻译Section4~6。
- AI 输出摘要：见 `drafts/section4-zh.tex`、`drafts/section5-zh.tex`、`drafts/section6-zh.tex`。

### 日期：2026-07-06

- 使用工具：Codex
- 提问内容：帮我翻译Section7。
- AI 输出摘要：见 `drafts/section7-zh.tex`。

### 日期：2026-07-07

- 使用工具：Codex
- 提问内容：帮我翻译Section8。
- AI 输出摘要：见 `drafts/section8-zh.tex`。

**注：2026-07-07调整目录后，上述目录全部变更为`drafts/paper-zh/...`**

### 日期：2026-07-08

- 使用工具：Codex
- 提问内容：帮我翻译Section9。
- AI 输出摘要：见 `drafts/paper-zh/section9-zh.tex`。

### 日期：2026-07-08

- 使用工具：网页端ChatGPT 5.5 Thinking
- 提问内容：请你帮我讲解finger search tree的原理
- AI 输出摘要：

Finger search tree 的核心思想可以一句话概括：

**普通平衡树每次从根开始搜，所以是 `O(log n)`；finger search tree 记住一个“手指”位置，从这个位置附近开始搜，所以如果目标离手指很近，只要 `O(log d)`。**

这里的 `d` 不是值差，而是**排名距离**：比如有序集合里上次在第 100 个元素，现在找第 106 个元素，那么 `d = 6`。经典 finger search tree 最早是 Guibas 等人在 B-tree 思路上提出的，可以做到距离为 `d` 的搜索 `O(log d)`；一些版本还支持在固定数量 finger 下常数级更新。([cs.au.dk][1])

---

## 1. “finger”到底是什么？

finger 就是一个指针，指向树中的某个元素，或者某个叶子位置。

比如有序序列：

```text
3  5  8  12  17  20  21  24  30  35
                  ↑
               finger
```

如果现在要找 `21` 或 `24`，它们离 `20` 很近，从 `20` 附近找显然比从整棵树根节点重新找更合理。

所以 finger search tree 想利用的是一种**局部性**：

> 最近访问过的位置附近，很可能还会继续被访问。

---

## 2. 为什么普通平衡树不够？

你可能会想：那普通 AVL、红黑树也可以从某个节点开始，往上爬，再往下找啊？

问题在于：**普通平衡树中，相邻元素的 LCA 可能非常高。**

比如：

```text
        root
       /    \
   很大左子树  很大右子树
```

左子树最大值和右子树最小值在有序序列里是相邻的，但它们的 LCA 是根。
所以即使两个元素排名距离 `d = 1`，从一个爬到另一个也可能要走 `O(log n)`。

因此真正的 finger search tree 不能只是“从节点往上找 LCA”。它需要额外结构，让你能从 finger 附近**按距离逐渐扩大搜索范围**。经典做法包括基于 B-tree / level-linked `(2,4)` tree 的结构。([cs.au.dk][1])

---

## 3. 核心结构：叶子存元素，内部节点存区间

可以把 finger search tree 粗略理解成一棵多叉平衡树：

```text
level 3:        [1 ... 32]
              /           \
level 2:  [1 ... 16]    [17 ... 32]
          /      \        /       \
level 1: ...    ...   [17...24]  [25...32]
                         |
level 0:          17 18 19 20 21 22 23 24
                                ↑
                             finger
```

每个内部节点代表一段连续有序区间。
同时，不同层的节点之间会有横向链接，方便你从 finger 附近往左或往右“扩张”。

重点是：**越高层的节点，覆盖的元素范围越大。**

可以近似理解为：

```text
第 0 层：覆盖 1 个元素
第 1 层：覆盖常数个元素
第 2 层：覆盖更多元素
第 3 层：覆盖约 2^3 级别的范围
第 k 层：覆盖约 2^k 级别的范围
```

所以如果目标离 finger 的排名距离是 `d`，那只要升到大约 `log d` 层，就足够覆盖它。

---

## 4. 搜索过程怎么做？

假设 finger 指向 `20`，现在要找 `26`：

```text
... 17 18 19 20 21 22 23 24 25 26 27 ...
             ↑                 ↑
           finger             target
```

搜索大致分两步。

第一步：**从 finger 开始向上扩张范围。**

```text
范围 1：只看 20
范围 2：看 19~21
范围 4：看 17~24
范围 8：看 17~32
```

发现 `26` 落在更大的区间里之后，就停下。

第二步：**从这个区间节点往下正常搜索。**

也就是在 `[17...32]` 这个小范围内往下找 `26`。

因为上升了 `O(log d)` 层，下降也只需要 `O(log d)` 层，所以总复杂度就是：

```text
O(log d)
```

这和数组里的“指数搜索”很像：从当前位置开始先跳 `1, 2, 4, 8, ...`，找到目标所在范围后再二分。文献里也常把 finger search 和这种“距离相关”的指数式扩张联系起来；在有序数组中，从 finger 出发做指数搜索可以达到约 `2 log d + O(1)` 次比较。([cs.au.dk][2])

---

## 5. 为什么是 `O(log d)`，不是 `O(d)`？

因为它不是一个一个元素往旁边扫，而是**一层一层扩大范围**。

如果目标距离是 `d = 64`，线性扫要 64 步。
但 finger search tree 的搜索范围大概这样扩张：

```text
1 → 2 → 4 → 8 → 16 → 32 → 64
```

只需要 6 次扩张。

所以复杂度是：

```text
log2(64) = 6
```

这就是 finger search tree 的关键直觉：
**离得近就很快，离得远最多退化到普通平衡树级别。**

---

## 6. 插入和删除怎么理解？

如果你已经有 finger，并且要在它附近插入：

```text
... 20 21 24 ...
       ↑
     finger
```

插入 `22`、`23` 这种附近元素时，不需要从根重新找插入位置。先用 finger search 找到相邻位置，然后在叶子附近局部修改。

经典 finger search tree 的一些版本可以在固定数量 finger 的前提下，支持 `O(log d)` 搜索、移动 finger `O(log d)`，并支持很快的更新；后续工作还研究了 worst-case 常数更新等更强版本。([cs.au.dk][1])

直觉上，插入删除之所以能快，是因为：

```text
搜索位置：O(log d)
局部修改：通常只影响附近少量节点
重平衡：通过 B-tree 式分裂/合并或增量维护解决
```

---

## 7. 和 Splay / Treap 的关系

严格来说，finger search tree 是一类数据结构/性质，不一定指某一种固定实现。

几种常见关系：

| 结构                        | 是否有 finger search 思想                        |
| ------------------------- | ------------------------------------------- |
| 经典 finger search tree     | 显式维护 finger，目标是 `O(log d)`                  |
| Level-linked `(2,4)` tree | 经典实现之一                                      |
| Treap                     | 可实现期望 `O(log d)` 的 finger search            |
| Skip list                 | 也可以用随机化方式支持 finger search                   |
| Splay tree                | 隐式 finger：上次访问的点会被伸展到根，满足 dynamic finger 性质 |

Aarhus 的综述提到，treap、skip list、level-linked `(2,4)` tree、splay tree 都可以从不同角度支持 finger search；其中 splay tree 的特殊点是它不显式保存 finger，而是把“上次访问的元素”作为隐式 finger。([cs.au.dk][1])

---

## 8. 一句话总结

finger search tree 的原理就是：

> 把有序元素组织成一棵带“局部入口”的平衡树。
> 查询时不从根开始，而是从 finger 附近开始，按层扩大搜索范围。
> 因为每升一层，覆盖范围按常数倍扩大，所以距离为 `d` 的查询只要 `O(log d)`。

你可以把它理解成：

```text
普通平衡树：从全局入口 root 搜索
finger search tree：从局部入口 finger 搜索
```

它优化的不是最坏情况下的 `n`，而是利用访问序列的局部性，把复杂度从：

```text
O(log n)
```

变成：

```text
O(log d)
```

其中 `d` 是目标和 finger 在有序序列里的排名距离。

[1]: https://cs.au.dk/~gerth/papers/finger05.pdf?utm_source=chatgpt.com "Finger Search Trees - Aarhus Universitet"
[2]: https://cs.au.dk/~gerth/papers/sosa25.pdf?utm_source=chatgpt.com "Pure Binary Finger Search Trees Pu - cs.au.dk"

### 日期：2026-07-09

- 使用工具：Codex
- 提问内容：帮我翻译Section10。
- AI 输出摘要：见 `drafts/paper-zh/section10-zh.tex`。

### 日期：2026-07-12

- 使用工具：Codex
- 提问内容：帮我翻译Section11。
- AI 输出摘要：见 `drafts/paper-zh/section11-zh.tex`。

### 日期：2026-07-15~2026-07-17

- 使用工具：网页端ChatGPT 5.6 Thinking
- 提问内容：梳理初读论文中记录在notes中的所有问题
- AI 输出摘要：太长了且过于零散，略。

### 日期：2026-07-18

- 使用工具：Codex
- 提问内容：帮我初始化一个review的tex目录
- AI 输出摘要：见 `drafts/review`（未生成实际内容，只生成了框架）。