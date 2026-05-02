# 动态规划学习笔记 (DP Notes)

---

## 目录

- [第一章：带路径的 DFS → 2D DP（位置依赖型）](#第一章带路径的-dfs--2d-dp位置依赖型)
  - [1.1 核心思想框架](#11-核心思想框架)
  - [1.2 递归 vs DP 方向对比](#12-递归-vs-dp-方向对比)
  - [1.3 空间优化：2D → 1D 滚动数组](#13-空间优化2d--1d-滚动数组)
  - [1.4 DP 复杂度分析](#14-dp-复杂度分析)
  - [1.5 题目详解：64 最小路径和（Minimum Path Sum）](#15-题目详解64-最小路径和minimum-path-sum)
  - [1.6 题目详解：79 单词搜索（Word Search）— 为什么不能用 DP](#16-题目详解79-单词搜索word-search为什么不能用-dp)
  - [1.7 题目详解：1143 最长公共子序列（LCS）](#17-题目详解1143-最长公共子序列lcs)
  - [1.8 题目详解：最长回文子序列](#18-题目详解最长回文子序列)
  - [1.9 题目详解：节点数为高度 n 不大于 m 的二叉树个数](#19-题目详解节点数为高度-n-不大于-m-的二叉树个数)
  - [1.10 题目详解：329 最长递增路径（Longest Increasing Path in a Matrix）](#110-题目详解329-最长递增路径longest-increasing-path-in-a-matrix)
  - [1.11 补充：最长回文子串 vs 最长回文子序列的区别](#111-补充最长回文子串-vs-最长回文子序列的区别)
- [第二章：2D DP 练习 + 字符串 DP + 完全背包](#第二章2d-dp-练习--字符串-dp--完全背包)
  - [2.1 题目练习：118 / 119 杨辉三角](#21-题目练习118--119-杨辉三角)
  - [2.2 题目练习：62 不同路径（Unique Paths）](#22-题目练习62-不同路径unique-paths)
  - [2.3 题目练习：63 不同路径 II（Unique Paths II）](#23-题目练习63-不同路径-ii)
  - [2.4 题目练习：64 最小路径和（空间优化）](#24-题目练习64-最小路径和空间优化)
  - [2.5 题目练习：79 单词搜索（DFS 回溯）](#25-题目练习79-单词搜索dfs-回溯)
  - [2.6 题目练习：1143 最长公共子序列](#26-题目练习1143-最长公共子序列)
  - [2.7 题目详解：72 编辑距离（Edit Distance）](#27-题目详解72-编辑距离edit-distance)
  - [2.8 题目详解：139 单词拆分（Word Break）— 完全背包](#28-题目详解139-单词拆分word-break--完全背包)
  - [2.9 DFS 回溯 vs DP 决策总结](#29-dfs-回溯-vs-dp-决策总结)

---

## 第一章：带路径的 DFS → 2D DP（位置依赖型）

### 1.1 核心思想框架

```
暴力递归（DFS）
    ↓ 发现重复子问题（overlapping subproblems）
记忆化搜索（Memoization）
    ↓ 改变思维方向：复杂 → 简单 变成 简单 → 复杂
递推 DP（Tabulation，Bottom-Up）
    ↓ 观察状态依赖方向，压缩冗余维度
空间优化 DP（滚动数组 / 1D DP）
```

**三个判断 DP 的关键问题：**

| 问题 | 说明 |
|------|------|
| 有没有重叠子问题？ | 同一个 `f(i,j)` 会被多次计算 |
| 有没有最优子结构？ | 当前最优解可以由子问题最优解推出 |
| 返回值能否用"位置"来表达？ | 可变参数的所有可能性能否用数组下标枚举 |

> **关键口诀**：
> - **DP** = 简单位置 → 复杂位置（Bottom-Up，从底到顶）
> - **递归** = 复杂位置 → 简单位置（Top-Down，从顶到底）

---

### 1.2 递归 vs DP 方向对比

| | 递归（DFS / Top-Down） | DP（Bottom-Up） |
|---|---|---|
| 方向 | 顶 → 底（复杂 → 简单） | 底 → 顶（简单 → 复杂） |
| 思维方式 | "我从哪来？" | "我能去哪？" |
| 起点 | 从目标出发，往回拆解 | 从 base case 出发，往前推 |
| 缺点 | 可能重复计算、栈溢出 | 需要找到正确的遍历顺序 |
| 空间 | O(递归深度) 栈空间 + cache | O(dp 数组大小) |

---

### 1.3 空间优化：2D → 1D 滚动数组

**什么时候可以压缩？**

> `dp[i][j]` 只依赖**上一行**和**同行左边** → 可以用一个 1D 数组滚动覆盖

```
dp[i][j] 依赖：
  dp[i-1][j]   ← 正上方（上一行）
  dp[i][j-1]   ← 左边（同行）

遍历方向从左到右，每次更新时：
  dp[j]（还没更新）= 上一行的 dp[i-1][j]  ✅
  dp[j-1]（已更新）= 当前行的 dp[i][j-1]  ✅
```

**两个滚动数组（更易理解）：**

```python
prev = [...]  # 上一行
curr = [...]  # 当前行
# 每行结束后：
prev, curr = curr, [初始值] * n  # 滚动交换
```

**一个数组原地更新（空间最优）：**

```python
dp = [...]  # 从左到右原地覆盖，保证顺序正确
```

---

### 1.4 DP 复杂度分析

```
时间复杂度 = 状态总数 × 每个状态的枚举代价
```

| 题型 | 状态数 | 每格枚举 | 总复杂度 |
|------|--------|----------|----------|
| 最小路径和 | O(M×N) | O(1)（只看上和左） | **O(MN)** |
| 最长公共子序列 | O(M×N) | O(1) | **O(MN)** |
| 背包问题 | O(N×W) | O(1) | **O(NW)** |
| 区间 DP（如戳气球）| O(N²) | O(N)（枚举分割点）| **O(N³)** |

> **可变参数的可能性相乘（xiangcheng）**：
> 两个可变参数 `i ∈ [0,M)`, `j ∈ [0,N)` → 状态总数 = M × N，再乘以每个状态的枚举代价。

---

### 1.5 题目详解：64 最小路径和（Minimum Path Sum）

**题目**：从 `grid[0][0]` 走到 `grid[m-1][n-1]`，只能向右或向下，求最小路径和。

**严格位置依赖（画图！）：**

```
dp[i][j] 只依赖：
  dp[i-1][j]  ← 正上方
  dp[i][j-1]  ← 左边

依赖箭头方向：↓ 和 →
遍历顺序：从上到下，从左到右 ↘
```

**Step 1：暴力递归**

```python
def minPathSum(grid):
    def dfs(i, j):
        if i == 0 and j == 0:
            return grid[0][0]
        if i < 0 or j < 0:
            return float('inf')
        return grid[i][j] + min(dfs(i-1, j), dfs(i, j-1))
    m, n = len(grid), len(grid[0])
    return dfs(m-1, n-1)
# ❌ 时间 O(2^(M+N))，大量重复计算
```

**Step 2：记忆化搜索**

```python
from functools import lru_cache

def minPathSum(grid):
    @lru_cache(maxsize=None)
    def dfs(i, j):
        if i == 0 and j == 0:
            return grid[0][0]
        if i < 0 or j < 0:
            return float('inf')
        return grid[i][j] + min(dfs(i-1, j), dfs(i, j-1))
    m, n = len(grid), len(grid[0])
    return dfs(m-1, n-1)
# ✅ 时间 O(MN)，空间 O(MN)
```

**Step 3：递推 DP（2D）**

```python
def minPathSum(grid):
    m, n = len(grid), len(grid[0])
    dp = [[0]*n for _ in range(m)]
    for i in range(m):
        for j in range(n):
            if i == 0 and j == 0:
                dp[i][j] = grid[0][0]
            elif i == 0:
                dp[i][j] = dp[i][j-1] + grid[i][j]
            elif j == 0:
                dp[i][j] = dp[i-1][j] + grid[i][j]
            else:
                dp[i][j] = grid[i][j] + min(dp[i-1][j], dp[i][j-1])
    return dp[m-1][n-1]
# ✅ 时间 O(MN)，空间 O(MN)
```

**Step 4：1D 滚动数组（空间优化）**

```python
def minPathSum(grid):
    m, n = len(grid), len(grid[0])
    dp = [float('inf')] * n
    for i in range(m):
        for j in range(n):
            if i == 0 and j == 0:
                dp[j] = grid[0][0]
            elif i == 0:
                dp[j] = dp[j-1] + grid[i][j]
            elif j == 0:
                dp[j] = dp[j] + grid[i][j]       # dp[j] 未更新 = 上一行
            else:
                dp[j] = min(dp[j], dp[j-1]) + grid[i][j]
    return dp[n-1]
# ✅ 时间 O(MN)，空间 O(N)
```

---

### 1.6 题目详解：79 单词搜索（Word Search）— 为什么不能用 DP

**为什么不能用 DP？**

| 条件 | 最小路径和 | 单词搜索 |
|------|-----------|---------|
| 路径可重复经过格子？ | ✅ 无所谓 | ❌ 不能重复 |
| 状态能用坐标表达？ | ✅ `dp[i][j]` 够了 | ❌ 还需记录已走格子集合 |
| 状态空间大小 | O(MN) | O(MN × 2^(MN))，爆炸！ |
| 重复子问题多吗？ | ✅ 很多 | ❌ 极少 |

> **重复子问题极少 → 记忆化收益为 0 → 只能暴力 DFS 回溯**

```python
def exist(board, word):
    m, n = len(board), len(board[0])

    def dfs(i, j, k):
        if k == len(word):
            return True
        if i < 0 or i >= m or j < 0 or j >= n:
            return False
        if board[i][j] != word[k]:
            return False
        tmp = board[i][j]
        board[i][j] = '#'          # 原地标记已访问
        found = (dfs(i+1,j,k+1) or dfs(i-1,j,k+1) or
                 dfs(i,j+1,k+1) or dfs(i,j-1,k+1))
        board[i][j] = tmp          # 回溯，恢复现场
        return found

    for i in range(m):
        for j in range(n):
            if dfs(i, j, 0):
                return True
    return False
```

---

### 1.7 题目详解：1143 最长公共子序列（LCS）

**dp 定义**：`dp[i][j]` = `s1` 前 `i` 个字符与 `s2` 前 `j` 个字符的 LCS 长度。

**状态转移：**

```
if s1[i-1] == s2[j-1]:   dp[i][j] = dp[i-1][j-1] + 1
else:                     dp[i][j] = max(dp[i-1][j], dp[i][j-1])
```

**完整代码（len+1 大小，自然覆盖边界）：**

```python
def longestCommonSubsequence(s1, s2):
    m, n = len(s1), len(s2)
    dp = [[0] * (n+1) for _ in range(m+1)]
    for i in range(1, m+1):
        for j in range(1, n+1):
            if s1[i-1] == s2[j-1]:
                dp[i][j] = dp[i-1][j-1] + 1
            else:
                dp[i][j] = max(dp[i-1][j], dp[i][j-1])
    return dp[m][n]
# ✅ 时间 O(MN)，空间 O(MN)
```

---

### 1.8 题目详解：最长回文子序列

**核心技巧**：最长回文子序列 = `s` 与 `reversed(s)` 的 LCS！

**区间 DP 做法：**

```
dp[i][j] = s[i..j] 的最长回文子序列长度

if s[i] == s[j]:   dp[i][j] = dp[i+1][j-1] + 2
else:              dp[i][j] = max(dp[i+1][j], dp[i][j-1])

遍历方向：按区间长度从小到大（先短后长）
  for length in range(2, n+1):
      for i in range(n - length + 1):
          j = i + length - 1
```

> **口诀**：区间 DP 先填短区间，再填长区间，因为长区间依赖短区间。

---

### 1.9 题目详解：节点数为高度 n 不大于 m 的二叉树个数

```
f(n, m) = Σ f(k, m-1) × f(n-1-k, m-1)    for k in 0..n-1

base case：
  f(0, m) = 1   （空树）
  f(n, 0) = 0   （高度为0但需要节点，不可能）
```

DP 版本：状态 `dp[n][m]`，两层循环枚举 m、n，内层枚举 k（左子树大小）。时间复杂度 O(N² × M)。

---

### 1.10 题目详解：329 最长递增路径（Longest Increasing Path in a Matrix）

**为什么不能用位置依赖 DP**：依赖方向由数值大小决定，不固定，无法确定遍历顺序。

**只能用记忆化搜索**（DAG 上 DFS，天然无环，可安全缓存）：

```python
def longestIncreasingPath(matrix):
    m, n = len(matrix), len(matrix[0])
    memo = {}
    def dfs(i, j):
        if (i, j) in memo:
            return memo[(i, j)]
        best = 1
        for di, dj in [(0,1),(0,-1),(1,0),(-1,0)]:
            ni, nj = i+di, j+dj
            if 0 <= ni < m and 0 <= nj < n and matrix[ni][nj] > matrix[i][j]:
                best = max(best, 1 + dfs(ni, nj))
        memo[(i, j)] = best
        return best
    return max(dfs(i, j) for i in range(m) for j in range(n))
```

---

### 1.11 补充：最长回文子串 vs 最长回文子序列的区别

| | 最长回文**子串** | 最长回文**子序列** |
|---|---|---|
| 要求 | 连续 | 不要求连续 |
| 典型题 | LC 5 | LC 516 |
| 经典解法 | 中心扩展法 / 区间 DP | 区间 DP / 转化为 LCS |
| 转移方程 | `s[i]==s[j] and dp[i+1][j-1]` | `s[i]==s[j]: dp[i+1][j-1]+2` |

---

## 第二章：2D DP 练习 + 字符串 DP + 完全背包

### 推荐学习顺序

```
70 爬楼梯 → 118 杨辉三角 → 119 杨辉三角 II
→ 62 不同路径 → 63 有障碍的路径 → 64 最小路径和
→ 79 单词搜索 → 1143 LCS → 72 编辑距离 → 139 单词拆分
```

---

### 2.1 题目练习：118 / 119 杨辉三角

#### LC 118 — 返回整个杨辉三角

**我出错的地方：**

1. 初始化要用 `range(1, numRows+1)`，不是 `range(numRows)`，否则会创建空数组
2. 内层循环要用 `range(1, i)`，不是 `range(1, i-1)`

```python
def generate(numRows):
    if numRows == 1:
        return [[1]]

    dp = [[1] * i for i in range(1, numRows + 1)]  # ⚠️ range(1, numRows+1)

    for i in range(1, numRows):     # i 是行索引（0-based），从第2行开始
        for j in range(1, i):       # ⚠️ range(1, i)，首尾已初始化为1，跳过
            dp[i][j] = dp[i-1][j-1] + dp[i-1][j]

    return dp
```

**索引心智模型：**

```
i=0: [1]           → 没有中间元素，range(1,0) 不执行
i=1: [1, 1]        → 没有中间元素，range(1,1) 不执行
i=2: [1, ?, 1]     → j=1，更新 dp[2][1]。range(1,2) → j=1 ✅
i=3: [1, ?, ?, 1]  → j=1,2。range(1,3) → j=1,2 ✅
```

---

#### LC 119 — 只返回第 rowIndex 行

**关键变化**：不需要保存整个 dp，只用 `prev`（上一行）和 `curr`（当前行）滚动。

**我出错的地方：**

1. 循环 `range(1, rowIndex+1)` 才能取到第 `rowIndex` 行
2. `curr` 长度是 `i+1`（第 i 行有 i+1 个元素）
3. `curr[j] = prev[j-1] + prev[j]`，用的是 prev（上一行）的下标

```python
def getRow(rowIndex):
    prev = [1]
    for i in range(1, rowIndex + 1):   # ⚠️ range(1, rowIndex+1)
        curr = [1] * (i + 1)           # ⚠️ 第i行有 i+1 个元素
        for j in range(1, i):          # 首尾是1，只更新中间
            curr[j] = prev[j-1] + prev[j]  # ⚠️ 左上prev[j-1] + 右上prev[j]
        prev = curr
    return prev
```

**索引对应关系图：**

```
prev（第i-1行）: [1,  3,  3,  1]
                  j-1  j
curr（第i行）:  [1,  ?,  ?,  ?,  1]
                      j
curr[j] = prev[j-1] + prev[j]
```

---

### 2.2 题目练习：62 不同路径（Unique Paths）

**题目**：m×n 网格，从左上角走到右下角，只能向右或向下，共有多少种走法？

**关键点**：`i, j` 从 1 开始遍历（第 0 行和第 0 列全为 1，初始化时设好）。

```python
def uniquePaths(m, n):
    dp = [[1] * n for _ in range(m)]   # 第0行、第0列全为1

    for i in range(1, m):
        for j in range(1, n):
            dp[i][j] = dp[i-1][j] + dp[i][j-1]

    return dp[m-1][n-1]
# 时间 O(MN)，空间 O(MN)
```

> **与杨辉三角的本质联系**：`dp[i][j] = C(i+j, i)`，每个格子就是组合数。

---

### 2.3 题目练习：63 不同路径 II

**题目**：在 62 的基础上，网格中有障碍物（值为 1），障碍物不能走。

**关键点**：

1. 起点有障碍直接返回 0
2. 第 0 行 / 第 0 列初始化时遇到障碍就 `break`（后面全部无法到达）
3. 主循环中障碍格子的 `dp` 值设为 0

```python
def uniquePathsWithObstacles(obstacleGrid):
    if obstacleGrid[0][0] == 1:
        return 0

    m, n = len(obstacleGrid), len(obstacleGrid[0])
    dp = [[0] * n for _ in range(m)]

    for i in range(n):          # 初始化第0行
        if obstacleGrid[0][i] != 1:
            dp[0][i] = 1
        else:
            break               # ⚠️ 必须 break，遇障后面全为0

    for j in range(m):          # 初始化第0列
        if obstacleGrid[j][0] != 1:
            dp[j][0] = 1
        else:
            break

    for i in range(1, m):
        for j in range(1, n):
            if obstacleGrid[i][j] == 1:
                dp[i][j] = 0
            else:
                dp[i][j] = dp[i-1][j] + dp[i][j-1]

    return dp[m-1][n-1]
```

> **为什么用 `break` 而不是 `continue`？**
> 第 0 行只能从左走，遇到障碍后右边所有格子都无法到达，必须全部保持 0。

---

### 2.4 题目练习：64 最小路径和（空间优化）

完整推导见第一章 1.5 节。核心口诀：

```
更新 dp[j] 时：
  dp[j]   （尚未更新）= 上一行同列的值  → 充当 dp[i-1][j]
  dp[j-1] （已经更新）= 当前行左边的值  → 充当 dp[i][j-1]
```

```python
def minPathSum(grid):
    m, n = len(grid), len(grid[0])
    dp = [float('inf')] * n
    for i in range(m):
        for j in range(n):
            if i == 0 and j == 0:
                dp[j] = grid[0][0]
            elif i == 0:
                dp[j] = dp[j-1] + grid[i][j]
            elif j == 0:
                dp[j] = dp[j] + grid[i][j]
            else:
                dp[j] = min(dp[j], dp[j-1]) + grid[i][j]
    return dp[n-1]
# ✅ 时间 O(MN)，空间 O(N)
```

---

### 2.5 题目练习：79 单词搜索（DFS 回溯）

完整分析见第一章 1.6 节。DFS 回溯三件套：

```
1. 终止条件    k == len(word) → True；越界或不匹配 → False
2. 标记当前格  board[i][j] = '#'（进入这条路）
3. 递归 + 恢复 found = dfs(上下左右)；board[i][j] = tmp（回溯）
```

**什么时候用回溯，什么时候用 DP：**

```
路径不确定，走错要回退        →  DFS 回溯（找路径 / 拼字符串 / 试所有可能）
路径会大量重复，有计数/最优解  →  DP
```

---

### 2.6 题目练习：1143 最长公共子序列

完整分析见第一章 1.7 节。本节总结**初始化的正确做法**：

**❌ 错误做法（手动找第一个匹配位置，逻辑复杂易出错）：**

```python
for i in range(s2):
    if text2[i] == text1[0]:
        dp[0][i] = 1
        break
for m in range(i+1, s2):
    dp[0][m] = 1
```

**✅ 正确做法：dp 大小开 (s1+1) × (s2+1)，index 0 代表空串**

```python
dp = [[0] * (s2+1) for _ in range(s1+1)]
# dp[0][j] = 0 自动表示"空串与任意串的 LCS = 0"

for i in range(1, s1+1):
    for j in range(1, s2+1):
        if text1[i-1] == text2[j-1]:   # ⚠️ i-1 和 j-1
            dp[i][j] = dp[i-1][j-1] + 1
        else:
            dp[i][j] = max(dp[i][j-1], dp[i-1][j])

return dp[s1][s2]
```

> **规律**：凡是字符串 DP，dp 数组长度开 `n+1`，
> `dp[i]` 对应字符串第 `i` 个字符，取值用 `s[i-1]`，永远不越界。

---

### 2.7 题目详解：72 编辑距离（Edit Distance）

**题目**：对 `word1` 进行最少的插入、删除、替换操作，变成 `word2`，求操作数。

**dp 定义**：`dp[i][j]` = 将 `word1` 前 `i` 个字符变成 `word2` 前 `j` 个字符的最少操作数。

**为什么 dp 大小是 (s1+1) × (s2+1)？**

> 必须包含空串：`dp[0][j] = j`（插入 j 次），`dp[i][0] = i`（删除 i 次）

**状态转移（三种操作）：**

```
if word1[i-1] == word2[j-1]:
    dp[i][j] = dp[i-1][j-1]               # 字符相同，无需操作

else:
    insert  = dp[i][j-1]   + 1            # word1[i] 后插入一个字符匹配 word2[j]
    delete  = dp[i-1][j]   + 1            # 删除 word1[i]
    replace = dp[i-1][j-1] + 1            # 将 word1[i] 替换为 word2[j]
    dp[i][j] = min(insert, delete, replace)
```

**我出错的地方：**

1. dp 大小要是 `(s1+1) × (s2+1)`，包含空串
2. 更新时用 `word1[i-1]` 和 `word2[j-1]`，因为 `i, j` 从 1 开始
3. insert 看的是 `dp[i][j-1]`，不是 `dp[i-1][j-1]`

```python
def minDistance(word1, word2):
    s1, s2 = len(word1), len(word2)
    dp = [[0] * (s2+1) for _ in range(s1+1)]

    for i in range(s2+1):   # 空串变成 word2 前j个字符：插入j次
        dp[0][i] = i
    for j in range(s1+1):   # word1 前i个字符变成空串：删除i次
        dp[j][0] = j

    for i in range(1, s1+1):
        for j in range(1, s2+1):
            if word1[i-1] == word2[j-1]:
                dp[i][j] = dp[i-1][j-1]
            else:
                insert  = dp[i][j-1]   + 1
                delete  = dp[i-1][j]   + 1
                replace = dp[i-1][j-1] + 1
                dp[i][j] = min(insert, delete, replace)

    return dp[s1][s2]
# ✅ 时间 O(MN)，空间 O(MN)
```

**dp 表格示例（word1="abd", word2="abc"）：**

```
            _  a  b  c
         _  0  1  2  3
         a  1  0  1  2
         b  2  1  0  1
         d  3  2  1  1  ← dp[3][3]=1，把d替换为c
```

---

### 2.8 题目详解：139 单词拆分（Word Break）— 完全背包

**题目**：给字符串 `s` 和单词词典 `wordDict`，判断 `s` 是否能被拆分为词典中的单词。

**为什么是完全背包？**

> 每个单词可以被使用多次（完全背包），`s` 的子串是"背包容量"，单词是"物品"。

**排列数 vs 组合数的遍历顺序：**

```
求组合数（顺序不重要）：先遍历物品，再遍历背包
求排列数（顺序重要）：  先遍历背包，再遍历物品   ← 本题！
```

> 本题 "abc" 拆分方式顺序 matters → **先遍历背包（位置 i），再遍历物品（分割点 j）**

**dp 定义**：`dp[i]` = `s` 的前 `i` 个字符能否被拆分。

```
dp[i] = True  当且仅当存在 j，使得：
    dp[j] == True   AND   s[j:i] ∈ wordDict
```

```python
def wordBreak(s, wordDict):
    wordSet = set(wordDict)
    dp = [False] * (len(s) + 1)
    dp[0] = True   # 空串可以被拆分

    for i in range(1, len(s) + 1):    # 先遍历背包（位置）
        for j in range(i):            # 枚举分割点 j
            if dp[j] and s[j:i] in wordSet:
                dp[i] = True
                break                 # 找到一个就可以 break

    return dp[len(s)]
# ✅ 时间 O(N² × L)，N=字符串长度，L=单词平均长度
```

**完全背包知识点对比：**

| | 完全背包 | 0-1 背包 |
|--|---------|---------|
| 物品使用次数 | 无限次 | 最多 1 次 |
| 内层遍历方向 | 正向（从小到大）| 反向（从大到小）|
| 本题对应 | ✅ 单词可重复使用 | — |

---

### 2.9 DFS 回溯 vs DP 决策总结

```
问题特征                               选择
──────────────────────────────────────────────────────
路径本身需要记录（矩阵不能重复走）      →  DFS 回溯（79 单词搜索）
重复子问题极少，状态空间爆炸            →  DFS 回溯
需要枚举所有可能路径                   →  DFS 回溯

有重叠子问题 + 最优子结构              →  DP
计数 / 最短 / 最长 / 是否可行          →  DP
状态可以用位置（坐标/下标）枚举        →  DP

带约束但有重叠子问题                   →  记忆化搜索（Memoization）
                                           （329 最长递增路径）
```

---

## 第三章：（待续）

> 下一章继续补充...

---

*最后更新：第二章完成*
