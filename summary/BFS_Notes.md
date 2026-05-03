# BFS 学习笔记 (BFS Notes)

---

## 目录

- [第一章：BFS 核心知识](#第一章bfs核心知识)
  - [1.1 核心思想](#11-核心思想)
  - [1.2 两个核心模板（必须背）](#12-两个核心模板必须背)
  - [1.3 BFS vs DFS 选择指南](#13-bfs-vs-dfs-选择指南)
  - [1.4 三大核心用途](#14-三大核心用途)
  - [1.5 例题详解](#15-例题详解)
  - [1.6 BFS 常见坑](#16-bfs-常见坑)
  - [1.7 推荐练习顺序](#17-推荐练习顺序)
  
- [第二章：BFS 进阶 — 最短路径 / 双端队列 / 0-1 BFS / 堆](#第二章bfs-进阶--最短路径--双端队列--0-1-bfs--堆)
  - [2.1 矩阵最短路径原理（图解）](#21-矩阵最短路径原理图解)
  - [2.2 Queue 结构详解：left / right 指针 vs deque](#22-queue-结构详解left--right-指针-vs-deque)
  - [2.3 题目：1162 尽可能远离陆地](#23-题目1162-尽可能远离陆地as-far-from-land-as-possible)
  - [2.4 什么问题可以用 BFS？判断准则](#24-什么问题可以用-bfs判断准则)
  - [2.5 0-1 BFS 与双端队列（Deque）](#25-0-1-bfs-与双端队列deque)
  - [2.6 题目：2290 移除障碍物最小数目](#26-题目2290-到达角落需要移除障碍物的最小数目)
  - [2.7 题目：1368 有效路径最小代价](#27-题目1368-使网格图至少有一条有效路径的最小代价)
  - [2.8 题目：407 接雨水 II（小根堆）](#28-题目407-接雨水-ii2d-接雨水小根堆)
  - [2.9 题目：126 单词接龙 II（BFS + DFS）](#29-题目126-单词接龙-iibfs-建图--dfs-还原路径)
  - [2.10 补充：1D 接雨水（双指针）](#210-补充1d-接雨水双指针)
  - [2.11 第二章总结：BFS 变体全景图](#211-第二章总结bfs-变体全景图)

- [第三章：BFS 练习题代码整理](#第三章bfs-练习题代码整理)
  - [3.1 LC 102 二叉树层序遍历](#31-lc-102-二叉树层序遍历)
  - [3.2 LC 107 层序遍历 II（自底向上）](#32-lc-107-层序遍历-ii自底向上)
  - [3.3 LC 103 锯齿形层序遍历](#33-lc-103-锯齿形层序遍历)
  - [3.4 LC 199 二叉树右视图](#34-lc-199-二叉树右视图)
  - [3.5 LC 111 二叉树最小深度](#35-lc-111-二叉树最小深度)
  - [3.6 LC 513 找树左下角的值](#36-lc-513-找树左下角的值)
  - [3.7 六道题对比总结](#37-六道题对比总结)

---

## 第一章：BFS 核心知识

### 1.1 核心思想

BFS 的本质是**按层扩散**——像往水里丢石头，波纹一圈圈向外扩。
用**队列（Queue）** 保证"先进先出"，从而保证同一层节点一定比下一层先处理。

```
起点
  ↓ 第1层（距离=1）
  ↓ 第2层（距离=2）
  ↓ 第3层（距离=3）
  ...
```

> **第一次到达某个节点时，走的步数一定是最少的。**
> 因为比它步数少的层已全部处理完，都没碰到它。

---

### 1.2 两个核心模板（必须背）

**模板一：基础 BFS**

```python
from collections import deque

def bfs(root):
    if not root:
        return

    queue = deque([root])      # 1. 初始节点入队
    visited = set([root])      # 2. 标记已访问（图题必须有，树可省略）

    while queue:               # 3. 队列非空就继续
        node = queue.popleft() # 4. 出队（FIFO，保证层序）

        # 5. 处理当前节点
        print(node)

        for neighbor in get_neighbors(node):
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)  # 6. 邻居入队
```

**模板二：按层处理（更重要！）**

```python
def bfs_by_level(root):
    queue = deque([root])
    level = 0

    while queue:
        level_size = len(queue)      # ⭐ 记录当前层的节点数

        for _ in range(level_size):  # 把这一层全部处理完
            node = queue.popleft()
            # 处理 node...

            for neighbor in get_neighbors(node):
                queue.append(neighbor)

        level += 1                   # 整层处理完，层数 +1

    return level
```

> **树 vs 图**：树不需要 `visited`（无环），图必须要（防止重复入队死循环）。

---

### 1.3 BFS vs DFS 选择指南

| 问题类型 | 选哪个 | 原因 |
|---------|--------|------|
| 最短路径 / 最少步数 | **BFS** | BFS 第一次到达即最短，DFS 不保证 |
| 能否到达（连通性）| BFS 或 DFS 都可 | 都能遍历全图 |
| 所有可能路径 | **DFS** | BFS 不记录完整路径 |
| 层序输出 / 按距离分层 | **BFS** | 天然按层扩散 |
| 空间受限（路径很深）| **BFS** | DFS 递归栈可能溢出 |

---

### 1.4 三大核心用途

| 用途 | 典型题 |
|------|--------|
| 层序遍历 | LC 102、103、107、199 |
| 最短路径 | LC 111、127、1091 |
| 连通性 / 多源扩散 | LC 200、994、286 |

---

### 1.5 例题详解

---

#### 例题 1：LC 102 二叉树层序遍历（Binary Tree Level Order Traversal）

**题目**：返回二叉树的层序遍历结果，每层节点放一个列表。

**思路**：BFS 按层处理模板，用 `level_size` 区分每层边界。

```python
from collections import deque

def levelOrder(root):
    if not root:
        return []

    result = []
    queue = deque([root])

    while queue:
        level_size = len(queue)   # 当前层节点数
        level = []

        for _ in range(level_size):
            node = queue.popleft()
            level.append(node.val)

            if node.left:  queue.append(node.left)
            if node.right: queue.append(node.right)

        result.append(level)

    return result
# 时间 O(N)，空间 O(N)
```

**变体**：
- LC 107：结果反转 → `result[::-1]`
- LC 103：锯齿遍历 → 奇数层正序，偶数层逆序，用 `flag` 控制
- LC 199：右视图 → 每层只取最后一个 `level[-1]`

---

#### 例题 2：LC 111 二叉树最小深度（Minimum Depth of Binary Tree）

**题目**：返回根节点到最近叶子节点的最短路径层数。

**为什么用 BFS 而不是 DFS**：BFS 第一次遇到叶子节点，深度一定最小。DFS 要遍历完再比较。

```python
from collections import deque

def minDepth(root):
    if not root:
        return 0

    queue = deque([(root, 1)])  # (节点, 当前深度)

    while queue:
        node, depth = queue.popleft()

        # 叶子节点：左右子树都为空
        if not node.left and not node.right:
            return depth        # ⭐ BFS 第一次到达叶子，即最小深度

        if node.left:  queue.append((node.left,  depth + 1))
        if node.right: queue.append((node.right, depth + 1))
# 时间 O(N)，空间 O(N)
```

---

#### 例题 3：LC 200 岛屿数量（Number of Islands）

**题目**：给一个由 `'1'`（陆地）和 `'0'`（海水）组成的矩阵，计算岛屿个数。

**思路**：遍历每个格子，遇到 `'1'` 就 BFS 把整个岛屿淹掉（标记为 `'0'`），岛屿数 +1。

```python
from collections import deque

def numIslands(grid):
    if not grid:
        return 0

    m, n = len(grid), len(grid[0])
    count = 0

    def bfs(r, c):
        queue = deque([(r, c)])
        grid[r][c] = '0'  # 原地标记，避免额外 visited 数组

        while queue:
            x, y = queue.popleft()
            for dx, dy in [(0,1),(0,-1),(1,0),(-1,0)]:
                nx, ny = x+dx, y+dy
                if 0 <= nx < m and 0 <= ny < n and grid[nx][ny] == '1':
                    grid[nx][ny] = '0'  # 入队前立刻标记！防止重复入队
                    queue.append((nx, ny))

    for i in range(m):
        for j in range(n):
            if grid[i][j] == '1':
                bfs(i, j)
                count += 1

    return count
# 时间 O(MN)，空间 O(min(M,N))
```

> **关键细节**：入队时立刻标记 `'0'`，不要等出队再标记。
> 否则同一个格子可能被多次加入队列，导致重复访问。

---

#### 例题 4：LC 994 腐烂的橘子（Rotting Oranges）

**题目**：新鲜橘子（1）、腐烂橘子（2）、空格（0）。每分钟腐烂橘子感染四邻居。求最少几分钟全部腐烂，不能全部腐烂返回 -1。

**思路**：**多源 BFS**——所有腐烂橘子同时作为起点入队，同步向外扩散。

```python
from collections import deque

def orangesRotting(grid):
    m, n = len(grid), len(grid[0])
    queue = deque()
    fresh = 0

    # 1. 统计新鲜橘子数，所有腐烂橘子同时入队
    for i in range(m):
        for j in range(n):
            if grid[i][j] == 2:
                queue.append((i, j, 0))  # (行, 列, 时间)
            elif grid[i][j] == 1:
                fresh += 1

    if fresh == 0:
        return 0

    time = 0
    while queue:
        x, y, t = queue.popleft()
        for dx, dy in [(0,1),(0,-1),(1,0),(-1,0)]:
            nx, ny = x+dx, y+dy
            if 0 <= nx < m and 0 <= ny < n and grid[nx][ny] == 1:
                grid[nx][ny] = 2
                fresh -= 1
                time = t + 1
                queue.append((nx, ny, t+1))

    return time if fresh == 0 else -1
# 时间 O(MN)，空间 O(MN)
```

> **多源 BFS 核心**：把多个起点同时放入队列，等价于从一个"超级源点"同时扩散，
> 保证每个点的感染时间最短。

---

#### 例题 5：LC 127 单词接龙（Word Ladder）

**题目**：给定起始词 `beginWord`，目标词 `endWord`，词典 `wordList`。
每次只能改变一个字母，改变后的词必须在词典中。求最短转换序列长度。

**思路**：把每个单词当节点，差一个字母的两词之间有边，求最短路径 → BFS。

```python
from collections import deque

def ladderLength(beginWord, endWord, wordList):
    wordSet = set(wordList)
    if endWord not in wordSet:
        return 0

    queue = deque([(beginWord, 1)])
    visited = {beginWord}

    while queue:
        word, steps = queue.popleft()

        for i in range(len(word)):
            for c in 'abcdefghijklmnopqrstuvwxyz':
                new_word = word[:i] + c + word[i+1:]

                if new_word == endWord:
                    return steps + 1

                if new_word in wordSet and new_word not in visited:
                    visited.add(new_word)
                    queue.append((new_word, steps + 1))

    return 0
# 时间 O(N × L × 26)，N=词典大小，L=词长度
```

---

#### 例题 6：LC 662 二叉树最大宽度（Maximum Width of Binary Tree）

**题目**：求二叉树每层中最左和最右非空节点之间的最大宽度（空节点也算在内）。

**思路**：BFS 层序 + 节点编号。父节点编号 `i` → 左子 `2i`，右子 `2i+1`。
每层宽度 = 最右编号 − 最左编号 + 1。

```python
from collections import deque

def widthOfBinaryTree(root):
    if not root:
        return 0

    max_width = 0
    queue = deque([(root, 0)])

    while queue:
        level_size = len(queue)
        _, first_idx = queue[0]   # 当前层最左编号
        last_idx = first_idx

        for _ in range(level_size):
            node, idx = queue.popleft()
            idx -= first_idx      # ⭐ 每层偏移，防止 2^n 溢出
            last_idx = idx

            if node.left:  queue.append((node.left,  2 * idx))
            if node.right: queue.append((node.right, 2 * idx + 1))

        max_width = max(max_width, last_idx + 1)

    return max_width
# 时间 O(N)，空间 O(N)
```

---

### 1.6 BFS 常见坑

| 坑 | 说明 | 解决 |
|----|------|------|
| 出队后才标记 visited | 同一节点被多次入队 | **入队时立刻标记** |
| 忘记初始化 visited | 起点被重复处理 | 初始化时把起点加入 visited |
| 单源 vs 多源混淆 | 多个起点未同时入队 | 初始化时把所有起点一次性全入队 |
| 层序时忘记 `level_size` | 无法区分层边界 | 每层开始前 `level_size = len(queue)` |
| 图题忘记 visited | 有环时死循环 | 图题必须有 visited |

---

### 1.7 推荐练习顺序

```
第一阶段（入门）
  LC 102  二叉树层序遍历       ← BFS 基础模板
  LC 107  层序遍历 II          ← 结果翻转
  LC 199  二叉树右视图         ← 每层取最后一个
  LC 111  二叉树最小深度       ← BFS 找最短

第二阶段（图上 BFS）
  LC 200  岛屿数量             ← 矩阵 BFS + 原地标记
  LC 994  腐烂的橘子           ← 多源 BFS

第三阶段（抽象图 + 技巧）
  LC 127  单词接龙             ← 抽象图最短路径
  LC 662  二叉树最大宽度       ← BFS + 编号技巧
  LC 103  锯齿层序遍历         ← BFS + 方向控制
```

---

## 第二章：BFS 进阶 — 最短路径 / 双端队列 / 0-1 BFS / 堆

### 目录（第二章）

- [2.1 矩阵最短路径原理（图解）](#21-矩阵最短路径原理图解)
- [2.2 Queue 结构详解：left / right 指针 vs deque](#22-queue-结构详解left--right-指针-vs-deque)
- [2.3 题目：1162 尽可能远离陆地（As Far from Land as Possible）](#23-题目1162-尽可能远离陆地as-far-from-land-as-possible)
- [2.4 什么问题可以用 BFS？判断准则](#24-什么问题可以用-bfs判断准则)
- [2.5 0-1 BFS 与双端队列（Deque）](#25-0-1-bfs-与双端队列deque)
- [2.6 题目：2290 到达角落需要移除障碍物的最小数目](#26-题目2290-到达角落需要移除障碍物的最小数目)
- [2.7 题目：1368 使网格图至少有一条有效路径的最小代价](#27-题目1368-使网格图至少有一条有效路径的最小代价)
- [2.8 题目：407 接雨水 II（2D 接雨水，小根堆）](#28-题目407-接雨水-ii2d-接雨水小根堆)
- [2.9 题目：126 单词接龙 II（BFS 建图 + DFS 还原路径）](#29-题目126-单词接龙-iibfs-建图--dfs-还原路径)
- [2.10 补充：1D 接雨水（双指针）](#210-补充1d-接雨水双指针)

---

### 2.1 矩阵最短路径原理（图解）

**问题**：在 0/1 矩阵中（0 可走，1 是障碍），从位置 A 到位置 E 的最短路径。

**核心思路**：把 A 当根节点，BFS 一圈圈向外扩，第一次到达 E 时的层数就是最短距离。

```
A 是源头，BFS 逐层扩散：

Layer 0:  A
Layer 1:  B, C, D         ← A 的邻居，距离 = 1
Layer 2:  E, H, F, G      ← B/C/D 的邻居，距离 = 2
                                         ↑ 第一次到达 E，答案是 2
```

**队列的演变过程（你的笔记补全版）：**

```
初始：queue = [(A, 0)]

Pop (A, 0)  → 加入邻居 B,C,D
queue = [(B,1), (C,1), (D,1)]

Pop (B, 1)  → 加入邻居 E,H
queue = [(C,1), (D,1), (E,2), (H,2)]

Pop (C, 1)  → 加入邻居 F
queue = [(D,1), (E,2), (H,2), (F,2)]

Pop (D, 1)  → 加入邻居 G
queue = [(E,2), (H,2), (F,2), (G,2)]

Pop (E, 2)  → E 是目标！返回 2  ✅
```

**关键设计要素：**

| 要素 | 说明 |
|------|------|
| `visited` 矩阵 | 防止重复入队，必须在**入队时**立刻标记 |
| 单点弹出 | 队列存 `(节点, 距离)`，弹出时判断是否到达终点 |
| 整层弹出 | 用 `level_size = len(queue)` 区分层，层数即距离 |
| 多源 BFS | 多个起点同时入队，等价于"超级源点" |

**任意两节点之间距离相等的特性**：

> BFS 的本质是所有边权为 1 的情况下的 Dijkstra。
> 在无权图（或等权图）中，BFS 保证第一次到达任何节点时，距离一定最短。

---

### 2.2 Queue 结构详解：left / right 指针 vs deque

**你的问题**：每个节点都有 left 和 right 吗？left 拿元素，right push 新元素？

**解答**：这里的 left/right 不是树节点的指针，而是**数组模拟队列时的两个游标**：

```
用数组模拟队列：
  queue = [B, C, D, E, H, F, G, ...]
           ↑                    ↑
         left（出队指针）     right（入队指针）

操作：
  出队（pop）：取 queue[left]，然后 left += 1
  入队（push）：在 queue[right] 写入，然后 right += 1
  队列非空条件：left < right
```

**用 Python deque 更简洁（推荐）：**

```python
from collections import deque
queue = deque()
queue.append(x)      # 右端入队（push）
queue.popleft()      # 左端出队（pop）
```

**整层弹出 vs 单点弹出对比：**

```python
# 方式一：单点弹出（存距离）
queue = deque([(start, 0)])
while queue:
    node, dist = queue.popleft()
    if node == target:
        return dist
    for neighbor in get_neighbors(node):
        if not visited[neighbor]:
            visited[neighbor] = True
            queue.append((neighbor, dist + 1))

# 方式二：整层弹出（用 level_size）
queue = deque([start])
level = 0
while queue:
    level_size = len(queue)          # 当前层大小
    for _ in range(level_size):
        node = queue.popleft()
        if node == target:
            return level
        for neighbor in get_neighbors(node):
            if not visited[neighbor]:
                visited[neighbor] = True
                queue.append(neighbor)
    level += 1
```

> **两种方式等价**，选哪种取决于题目是否需要按层处理其他信息。

---

### 2.3 题目：1162 尽可能远离陆地（As Far from Land as Possible）

**题目**：m×n 矩阵，0 是海洋，1 是陆地。找一个海洋格子，使它离最近的陆地最远，返回该最大距离（曼哈顿距离）。

**思路**：**多源 BFS**，所有陆地格子同时作为起点入队，向海洋扩散。
最后一个被扩散到的海洋格子，就是离陆地最远的点。

**复杂度**：O(M×N)，每个格子最多入队一次，每次处理四个方向，共 4 × M × N 次操作。

```python
from collections import deque

def maxDistance(grid):
    m, n = len(grid), len(grid[0])
    queue = deque()
    visited = [[False] * n for _ in range(m)]

    # 所有陆地同时入队（多源 BFS）
    for i in range(m):
        for j in range(n):
            if grid[i][j] == 1:
                queue.append((i, j))
                visited[i][j] = True

    # 全是陆地或全是海洋，无法计算
    if len(queue) == 0 or len(queue) == m * n:
        return -1

    dist = -1
    while queue:
        level_size = len(queue)
        dist += 1                          # 每扩散一层，距离 +1
        for _ in range(level_size):
            x, y = queue.popleft()
            for dx, dy in [(0,1),(0,-1),(1,0),(-1,0)]:
                nx, ny = x+dx, y+dy
                if 0 <= nx < m and 0 <= ny < n and not visited[nx][ny]:
                    visited[nx][ny] = True
                    queue.append((nx, ny))

    return dist
# 时间 O(MN)，空间 O(MN)
```

**你笔记里提到的实现细节：**

```
visited = bool 数组，防止重复入队
while l < r → 等价于 while queue（队列非空）
整层弹出 → 用 level_size 控制
```

---

### 2.4 什么问题可以用 BFS？判断准则

**你的问题**：可以理解为，如果能转化为层数（步数）问题就可以用 BFS？

**解答**：对，更精确的判断准则是：

```
适合 BFS 的三个特征：

1. 所有边的"代价"相同（等权图，或无权图）
   → 边权都是 1 时，BFS 层数 = 最短路径

2. 需要找最短路径 / 最少步数 / 最少操作数
   → BFS 保证第一次到达即最短

3. 状态可以用"节点"表示，并且可以枚举邻居
   → 节点可以是坐标、字符串、状态元组等
```

**不适合 BFS 的情况：**

```
边权不等（有 0 和 1 混合）→ 0-1 BFS（双端队列）
边权任意整数              → Dijkstra（优先队列）
需要记录完整路径          → DFS 回溯
```

**贴纸拼单词（Stickers to Spell Word）**：

> 每次使用一张贴纸 = 走一步，层数 = 最少贴纸数。可以 BFS，但状态空间大，
> 需要**剪枝**（已访问的状态不重复入队）。也可以用 DP（bitmask DP 更高效）。

---

### 2.5 0-1 BFS 与双端队列（Deque）

**你的问题**：01 BFS 的原理？为什么普通 BFS 不够用？

**为什么不能用普通 BFS？**

> 普通 BFS 假设所有边权为 1。
> 当边权混合了 0 和 1 时，"走一步"的代价不相同，
> BFS 的层数不再等于最短距离。

**0-1 BFS 原理：双端队列（Deque）**

```
核心思想：
  走权重为 0 的边 → 代价不变 → 加入队列头部（相当于没走一步）
  走权重为 1 的边 → 代价 +1  → 加入队列尾部（正常 BFS）

这样队列始终保持"代价从小到大"的顺序，
等价于 Dijkstra，但更高效（O(V+E) vs O((V+E)logV)）。
```

**0-1 BFS 模板：**

```python
from collections import deque

def bfs_01(graph, start, target):
    INF = float('inf')
    dist = [INF] * len(graph)
    dist[start] = 0
    dq = deque([start])

    while dq:
        x = dq.popleft()          # 从头部弹出（代价最小的节点）

        if x == target:
            return dist[x]

        for y, w in graph[x]:     # 遍历 x 的每条边，w ∈ {0, 1}
            new_dist = dist[x] + w
            if new_dist < dist[y]:
                dist[y] = new_dist
                if w == 0:
                    dq.appendleft(y)   # ⭐ 权重0 → 加头部
                else:
                    dq.append(y)       # ⭐ 权重1 → 加尾部

    return dist[target]
# 时间 O(V + E)，空间 O(V)
```

**你笔记的完整解析：**

```
dist[i] = 从原点到 i 的最短距离，初始化为 infinity
双端队列，原点进入，dist[原点] = 0

弹出 X（从头部）：
  若 X 是目标 → 返回 dist[X]
  若不是 → 考虑从 X 出发的每条边（权重 w）

  若 dist[Y] > dist[X] + w：  ← 发现更短路径，需要更新
      dist[Y] = dist[X] + w
      若 w == 0 → 加入队列头部（代价不变，优先处理）
      若 w == 1 → 加入队列尾部（代价+1，正常排队）
  否则 → 忽略（已有更短路径）

队列为空 → 结束
```

---

### 2.6 题目：2290 到达角落需要移除障碍物的最小数目

**题目**：m×n 网格，0 可以走，1 是障碍物（移除代价为 1）。
从左上角 `(0,0)` 到右下角 `(m-1,n-1)`，求需要移除的最少障碍物数。

**关键**：走到空格代价 0，走到障碍代价 1 → **0-1 BFS**。

```python
from collections import deque

def minimumObstacles(grid):
    m, n = len(grid), len(grid[0])
    INF = float('inf')
    dist = [[INF] * n for _ in range(m)]
    dist[0][0] = 0
    dq = deque([(0, 0)])           # (行, 列)

    while dq:
        x, y = dq.popleft()

        for dx, dy in [(0,1),(0,-1),(1,0),(-1,0)]:
            nx, ny = x+dx, y+dy
            if 0 <= nx < m and 0 <= ny < n:
                w = grid[nx][ny]   # 0 或 1
                new_dist = dist[x][y] + w
                if new_dist < dist[nx][ny]:
                    dist[nx][ny] = new_dist
                    if w == 0:
                        dq.appendleft((nx, ny))  # 权重0，头部
                    else:
                        dq.append((nx, ny))      # 权重1，尾部

    return dist[m-1][n-1]
# 时间 O(MN)，空间 O(MN)
```

---

### 2.7 题目：1368 使网格图至少有一条有效路径的最小代价

**题目**：m×n 网格，每格有一个箭头方向（右/左/下/上）。
沿箭头走代价 0，改变箭头方向代价 1，求从 `(0,0)` 到 `(m-1,n-1)` 的最小代价。

**关键**：沿原方向走 = 边权 0；改方向走 = 边权 1 → **0-1 BFS**。

```python
from collections import deque

def minCost(grid):
    m, n = len(grid), len(grid[0])
    # 方向映射：1右 2左 3下 4上
    dirs = {1:(0,1), 2:(0,-1), 3:(1,0), 4:(-1,0)}
    INF = float('inf')
    dist = [[INF] * n for _ in range(m)]
    dist[0][0] = 0
    dq = deque([(0, 0)])

    while dq:
        x, y = dq.popleft()
        # 四个方向都尝试
        for d, (dx, dy) in dirs.items():
            nx, ny = x+dx, y+dy
            if 0 <= nx < m and 0 <= ny < n:
                # 若当前格箭头指向此方向，代价0；否则代价1
                w = 0 if grid[x][y] == d else 1
                new_dist = dist[x][y] + w
                if new_dist < dist[nx][ny]:
                    dist[nx][ny] = new_dist
                    if w == 0:
                        dq.appendleft((nx, ny))
                    else:
                        dq.append((nx, ny))

    return dist[m-1][n-1]
# 时间 O(MN)，空间 O(MN)
```

---

### 2.8 题目：407 接雨水 II（2D 接雨水，小根堆）

**题目**：m×n 矩阵，每格是高度值，求矩阵能接多少体积的雨水。

**你的笔记**：`heap(i, j, 水线，小根堆)`，最薄弱的点进入。

**核心思想：木桶原理**

> 水位由最低的"边界"决定。先从最外圈最矮的格子开始，
> 向内扩散，维护当前"水线"（已知可以保住水的高度下限）。

**算法步骤：**

```
1. 把所有边界格子加入最小堆（按高度排序）
2. 弹出堆中最小高度的格子 (i, j, water_level)
3. 对其四个邻居：
   - 若邻居高度 < water_level → 可以积水，积水量 = water_level - 邻居高度
   - 邻居进入堆时，水线 = max(water_level, 邻居高度)
4. 重复直到堆为空
```

```python
import heapq

def trapRainWater(heightMap):
    if not heightMap or len(heightMap) < 3 or len(heightMap[0]) < 3:
        return 0

    m, n = len(heightMap), len(heightMap[0])
    visited = [[False] * n for _ in range(m)]
    heap = []

    # 把所有边界格子加入堆
    for i in range(m):
        for j in range(n):
            if i == 0 or i == m-1 or j == 0 or j == n-1:
                heapq.heappush(heap, (heightMap[i][j], i, j))
                visited[i][j] = True

    total = 0
    while heap:
        water_level, x, y = heapq.heappop(heap)  # 弹出最薄弱的边界点

        for dx, dy in [(0,1),(0,-1),(1,0),(-1,0)]:
            nx, ny = x+dx, y+dy
            if 0 <= nx < m and 0 <= ny < n and not visited[nx][ny]:
                visited[nx][ny] = True
                h = heightMap[nx][ny]

                if h < water_level:
                    total += water_level - h    # 积水

                # 水线取 max：邻居进堆时的水线至少和当前一样高
                heapq.heappush(heap, (max(water_level, h), nx, ny))

    return total
# 时间 O(MN log MN)，空间 O(MN)
```

**为什么用堆而不是普通 BFS？**

```
普通 BFS：按入队顺序处理，无法保证总是处理"最薄弱"的边界点
小根堆：始终弹出当前最低的边界，符合木桶原理
（类似 Dijkstra：优先处理代价最小的节点）
```

---

### 2.9 题目：126 单词接龙 II（Word Ladder II）

**题目**：从 `beginWord` 变到 `endWord`，每次改变一个字母，求所有最短路径。

**你的笔记**：`begin → end 一层一层 BFS 建图，与 DFS 结合，建立返图`。

**算法分两步：**

#### Step 1：BFS 建图（找最短层数 + 建反向邻接图）

```
从 beginWord 出发，BFS 逐层扩散。
记录每个节点从哪些节点可以到达（反向图），用于 DFS 还原路径。
当第一次到达 endWord 时，记录层数，继续处理完当前层，然后停止。

剪枝设计：
  - 已访问的词不再入队（防止重复）
  - 到达 endWord 后停止继续扩散（不需要更深的层）
```

```python
from collections import deque, defaultdict

def findLadders(beginWord, endWord, wordList):
    wordSet = set(wordList)
    if endWord not in wordSet:
        return []

    # BFS 建反向图：graph[word] = 上一层能到达 word 的所有词
    graph = defaultdict(list)
    layer = {beginWord}          # 当前层的所有词
    found = False

    while layer and not found:
        wordSet -= layer          # 从词典中移除当前层（剪枝）
        next_layer = set()

        for word in layer:
            for i in range(len(word)):
                for c in 'abcdefghijklmnopqrstuvwxyz':
                    new_word = word[:i] + c + word[i+1:]
                    if new_word in wordSet:
                        next_layer.add(new_word)
                        graph[new_word].append(word)  # 反向图：new_word ← word
                        if new_word == endWord:
                            found = True

        layer = next_layer

    if not found:
        return []

    # Step 2：DFS 从 endWord 沿反向图回溯所有最短路径
    result = []
    path = [endWord]

    def dfs(word):
        if word == beginWord:
            result.append(path[::-1])  # 反转路径
            return
        for prev in graph[word]:
            path.append(prev)
            dfs(prev)
            path.pop()

    dfs(endWord)
    return result
# 时间 O(N × L × 26 + 路径数 × 路径长度)
```

**为什么要建反图？**

```
BFS 从 beginWord 往 endWord 走（正向）
DFS 从 endWord 往 beginWord 回溯（反向）

正向 BFS：找最短层数，记录"谁能到达我"（反向边）
反向 DFS：沿反向边回溯，还原所有最短路径
```

**与 Word Ladder I 的区别：**

| | Word Ladder I（LC 127）| Word Ladder II（LC 126）|
|---|---|---|
| 要求 | 最短路径长度 | 所有最短路径 |
| 算法 | 单纯 BFS | BFS 建图 + DFS 回溯 |
| 难点 | 无 | 剪枝 + 反向图 + DFS |

---

### 2.10 补充：1D 接雨水（双指针）

**题目（LC 42）**：给一维高度数组，计算能接多少雨水。

**双指针思路（O(N) 时间，O(1) 空间）：**

```
每个位置能接的水 = min(左边最高, 右边最高) - 当前高度

用双指针从两端向中间夹逼：
  维护 left_max（左边已扫描到的最大高度）
  维护 right_max（右边已扫描到的最大高度）

  若 left_max < right_max：
      左指针处的积水由 left_max 决定（右边一定更高）
      water += left_max - height[left]
      left += 1

  否则：
      右指针处的积水由 right_max 决定
      water += right_max - height[right]
      right -= 1
```

```python
def trap(height):
    left, right = 0, len(height) - 1
    left_max = right_max = 0
    water = 0

    while left < right:
        left_max  = max(left_max,  height[left])
        right_max = max(right_max, height[right])

        if left_max < right_max:
            water += left_max - height[left]
            left += 1
        else:
            water += right_max - height[right]
            right -= 1

    return water
# 时间 O(N)，空间 O(1)
```

**1D vs 2D 接雨水对比：**

| | 1D 接雨水（LC 42）| 2D 接雨水（LC 407）|
|---|---|---|
| 结构 | 一维数组 | 二维矩阵 |
| 算法 | 双指针 | 小根堆（BFS 变体）|
| 时间 | O(N) | O(MN log MN) |
| 空间 | O(1) | O(MN) |
| 核心思想 | 两端夹逼，取较小边 | 木桶原理，从最薄弱边界向内扩 |

---

### 2.11 第二章总结：BFS 变体全景图

```
等权图（边权全为1）
  单源    →  普通 BFS（queue）
  多源    →  多源 BFS（所有起点同时入队）

0/1 混合权图
  边权 ∈ {0,1}  →  0-1 BFS（双端队列 deque）

任意权图
  边权 ≥ 0      →  Dijkstra（优先队列 heap）
  含负权边      →  Bellman-Ford

特殊变体
  求所有最短路径  →  BFS 建反图 + DFS 回溯（LC 126）
  2D 接雨水      →  小根堆 + BFS 扩散（LC 407）
  按层返回结果   →  BFS + level_size（LC 102、1162）
```

---

## 第三章：BFS 练习题代码整理

### 目录（第三章）

- [3.1 LC 102 二叉树层序遍历](#31-lc-102-二叉树层序遍历)
- [3.2 LC 107 层序遍历 II（自底向上）](#32-lc-107-层序遍历-ii自底向上)
- [3.3 LC 103 锯齿形层序遍历](#33-lc-103-锯齿形层序遍历)
- [3.4 LC 199 二叉树右视图](#34-lc-199-二叉树右视图)
- [3.5 LC 111 二叉树最小深度](#35-lc-111-二叉树最小深度)
- [3.6 LC 513 找树左下角的值](#36-lc-513-找树左下角的值)
- [3.7 六道题对比总结](#37-六道题对比总结)

---

### 3.1 LC 102 二叉树层序遍历

**题目**：返回每层节点值，每层放一个列表。

**关键点**：
- `if not root: return []` 必须放在 `deque` 初始化之前
- `level_size = len(q)` 在每层开始时固定住当前层的节点数
- `if node.left / if node.right` 自动过滤掉 None，不会把空节点加进队列

```python
def levelOrder(self, root):
    if not root:
        return []

    from collections import deque
    q = deque([root])
    res = []

    while q:
        level_size = len(q)        # 固定当前层节点数
        level_val = []

        for _ in range(level_size):
            node = q.popleft()
            level_val.append(node.val)

            if node.left:
                q.append(node.left)
            if node.right:
                q.append(node.right)

        res.append(level_val)

    return res
# 时间 O(N)，空间 O(N)
```

---

### 3.2 LC 107 层序遍历 II（自底向上）

**题目**：和 102 一样，但结果从最底层到根节点顺序返回。

**关键点**：BFS 过程完全不变，最后翻转 `res`。

两种翻转方式都对，选一种记住：

```python
# 方式一：切片翻转（简洁）
return res[::-1]

# 方式二：双指针原地翻转（你的写法，更底层）
left, right = 0, len(res) - 1
while left <= right:
    res[left], res[right] = res[right], res[left]
    left += 1
    right -= 1
return res
```

**完整代码：**

```python
def levelOrderBottom(self, root):
    if not root:
        return []

    from collections import deque
    q = deque([root])
    res = []

    while q:
        level_size = len(q)
        level_ans = []

        for _ in range(level_size):
            node = q.popleft()
            level_ans.append(node.val)
            if node.left:  q.append(node.left)
            if node.right: q.append(node.right)

        res.append(level_ans)

    return res[::-1]    # ← 唯一区别
# 时间 O(N)，空间 O(N)
```

---

### 3.3 LC 103 锯齿形层序遍历

**题目**：第 0 层从左到右，第 1 层从右到左，交替输出。

**关键点**：BFS 过程不变，只在收集结果时判断奇偶层。

```python
def zigzagLevelOrder(self, root):
    if not root:
        return []

    from collections import deque
    q = deque([root])
    res = []
    n = 0                          # 层计数器

    while q:
        level_size = len(q)
        level = []

        for _ in range(level_size):
            node = q.popleft()
            level.append(node.val)
            if node.left:  q.append(node.left)
            if node.right: q.append(node.right)

        if n % 2 == 0:
            res.append(level)      # 偶数层：正序
        else:
            res.append(level[::-1])  # 奇数层：翻转

        n += 1

    return res
# 时间 O(N)，空间 O(N)
```

**另一种等价写法（用 `bool` 更语义化）：**

```python
left_to_right = True
while q:
    ...
    res.append(level if left_to_right else level[::-1])
    left_to_right = not left_to_right
```

---

### 3.4 LC 199 二叉树右视图

**题目**：返回从右边看到的每层最右边的节点值。

**关键点**：每层最后一个弹出的节点（`i == level_size - 1`）就是最右边的节点。

```python
def rightSideView(self, root):
    if not root:
        return []

    from collections import deque
    q = deque([root])
    res = []

    while q:
        level_size = len(q)

        for i in range(level_size):
            node = q.popleft()

            if i == level_size - 1:    # ← 每层最后一个
                res.append(node.val)

            if node.left:  q.append(node.left)   # ⚠️ 顺序不能变
            if node.right: q.append(node.right)  # 先左后右，最后弹出的才是最右

    return res
# 时间 O(N)，空间 O(N)
```

**注意入队顺序**：先 left 后 right，队列里同层从左到右排列，最后一个弹出的才是最右节点。

> 等价写法：先 right 后 left 入队，改取 `i == 0`，效果完全相同，只是镜像。

---

### 3.5 LC 111 二叉树最小深度

**题目**：返回根节点到最近叶子节点的最短路径层数。

**关键点**：
- `level` 初始化为 `1`（根节点本身就是第一层）
- 叶子节点判断：`not (node.left or node.right)` 等价于 `not node.left and not node.right`
- BFS 第一次碰到叶子节点时直接返回，保证最短
- 最后的 `return level` 是死代码（不可能到达），但保留不影响正确性

```python
def minDepth(self, root):
    if not root:
        return 0

    from collections import deque
    q = deque([root])
    level = 1                      # 根节点本身是第一层

    while q:
        level_size = len(q)

        for _ in range(level_size):
            node = q.popleft()

            if not node.left and not node.right:
                return level       # ⭐ 第一次到达叶子，即最小深度

            if node.left:  q.append(node.left)
            if node.right: q.append(node.right)

        level += 1

    return level                   # 死代码，实际不会执行
# 时间 O(N)，空间 O(N)
```

**为什么不能用 DFS？**

```
DFS 会一路走到最深处才开始比较，必须遍历全树。
BFS 第一次碰到叶子就返回，可能只遍历一小部分。
→ 最坏情况一样，但最优情况 BFS 快得多。
```

---

### 3.6 LC 513 找树左下角的值

**题目**：找最底层最左边节点的值。

**关键点**：
- 先左后右入队，每层第一个弹出的（`i == 0`）就是最左边节点
- 用 `val` 不断覆盖，最后一次覆盖的就是最底层的值
- 最终返回 `val` 而不是 `node.val`（循环结束后 `node` 指向最后一个弹出的节点，不是最左）

```python
def findBottomLeftValue(self, root):
    if not root:
        return None

    from collections import deque
    q = deque([root])
    val = 0                        # 存最左节点的值

    while q:
        level_size = len(q)

        for i in range(level_size):
            node = q.popleft()

            if i == 0:             # ← 每层第一个 = 最左边
                val = node.val     # 不断覆盖，最后一次 = 最底层

            if node.left:  q.append(node.left)
            if node.right: q.append(node.right)

    return val
# 时间 O(N)，空间 O(N)
```

---

### 3.7 六道题对比总结

| 题目 | 核心变化点 | 关键代码 |
|------|-----------|---------|
| **102 层序遍历** | 每层收集所有节点 | `res.append(level_val)` |
| **107 自底向上** | 结果翻转 | `return res[::-1]` |
| **103 锯齿遍历** | 奇数层翻转 | `level[::-1] if n%2!=0` |
| **199 右视图** | 每层取最后一个 | `if i == level_size - 1` |
| **111 最小深度** | 第一次到叶子就返回 | `if not node.left and not node.right: return level` |
| **513 左下角** | 每层取第一个，持续覆盖 | `if i == 0: val = node.val` |

**共同骨架（六道题的 BFS 框架完全一致）：**

```python
if not root: return []
q = deque([root])

while q:
    level_size = len(q)
    for i in range(level_size):     # 或 for _ in range(level_size)
        node = q.popleft()
        # ← 唯一的变化点在这里
        if node.left:  q.append(node.left)
        if node.right: q.append(node.right)
    # ← 或者变化点在这里（层结束后处理）
```

> **规律**：掌握这一个骨架，六道题换的只是"在哪个位置收集结果"。

---

*最后更新：第三章完成*