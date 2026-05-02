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

## 第二章：（待续）

> 下一章继续补充...

---

*最后更新：第一章完成*
