
# LeetCode Practice Log 

--- 
## LeetCode Count (Daily Summary)

| Date | Easy | Medium | Hard | Total | Notes |
|------|------|--------|------|-------|------|
| 2026-04-17  | 2 | 1 | 0 | 3 | focused on array |

| 2026-04-18  |  5  + 2|  2 | 0  | 9  | 7 Listnode  + 2 array|
| 2026-04-19  |  2 |  3  |  1  |   | 6 merge sorting + stack |
| 2026-04-20  |   |   |   |   | |
| 2026-04-21  |   |   |   |   | |
| 2026-04-22  |   |   |   |   |  |
 

⭐ → 秒杀
⭐⭐ → 小问题
⭐⭐⭐ → 没思路
⭐⭐⭐⭐ → 看答案
⭐⭐⭐⭐⭐ → 



| Status | #   | Problem                                | Difficulty | Pattern                 | ⭐ Confidence | Notes           |
| ------ | --- | -------------------------------------- | ---------- | ----------------------- | ------------ | --------------- |
| ✔      | 509 | Fibonacci Number                       | Easy       | Linear DP / Recurrence  | ⭐⭐⭐⭐⭐        | 最基础 DP + 空间优化模板 |
| ✔      | 70  | Climbing Stairs                        | Easy       | Fibonacci Variant       | ⭐⭐⭐⭐⭐        | 经典“跳台阶”         |
| ✔      | 198 | House Robber（臭鼬问题）                     | Medium     | Take / Not Take         | ⭐⭐⭐⭐⭐        | 相邻约束 DP         |
| ✔      | 213 | House Robber II                        | Medium     | Circular DP             | ⭐⭐⭐⭐         | 环形数组（首尾不能同时取）   |
| ✔      | 91  | Decode Ways                            | Medium     | String DP               | ⭐⭐⭐⭐⭐        | 单/双字符拆分         |
| ✔      | 639 | Decode Ways II                         | Hard       | Combinatorial DP        | ⭐⭐⭐⭐         | `*` 多情况分类       |
| ✔      | 115 | Distinct Subsequences                  | Hard       | Subsequence DP          | ⭐⭐⭐⭐⭐        | 二维DP + 1D优化     |
| ✔      | 940 | Distinct Subsequences II               | Hard       | Subsequence Counting DP | ⭐⭐⭐⭐         | 去重子序列计数         |
| ✔      | 983 | Minimum Cost For Tickets               | Medium     | Cost DP (interval jump) | ⭐⭐⭐⭐⭐        | 1/7/30天跳转       |
| ✔      | 467 | Unique Substrings in Wraparound String | Medium     | DP by character ending  | ⭐⭐⭐⭐         | 字符结尾最长连续串       |






---

## Merge sorting, divide and conquer 
| Status | # | Problem | Difficulty | Pattern | ⭐ Confidence | Notes |
|--------|---|--------|-----------|---------|--------------|------|
| ✅ | 912 | Sort an Array | Medium | Merge Sort / Divide & Conquer | ⭐ | 基础归并排序，必须手写 |

| X | 88 | Merge Sorted Array | Easy | Merge / Two Pointers | ⭐⭐ | 从后往前填 |
| X | 148 | Sort List | Medium | Merge Sort / Linked List / Fast-Slow | ⭐⭐  | 链表归并排序🔥 |
| ✅| 23 | Merge k Sorted Lists | Hard | Divide & Conquer / Heap | ⭐ | 两两 merge 或 heap |
| X | 53 | Maximum Subarray | Medium | Divide & Conquer | ⭐⭐⭐ | 分治思路理解( using the pointer for the comparison cross left and right part) and it can use the prefix  / DP / Kadane  |

| ✅ | 169 | Majority Element | Easy | Divide & Conquer | ⭐ | 左右 majority 合并 |
| ☐ | 315 | Count of Smaller Numbers After Self | Hard | Merge Sort + Counting | ⭐ | OA 高频🔥 |
| ☐ | 493 | Reverse Pairs | Hard | Merge Sort + Counting | ⭐ | 经典变形🔥 |
| ☐ | 327 | Count of Range Sum | Hard | Prefix Sum + Merge Sort | ⭐ | 面试高频 |



## Stack 
| Status | #   | Problem                        | Difficulty | Pattern | ⭐ Confidence | Notes |
| ------ | --- | ------------------------------ | ---------- | ------- | ------------ | ----- |
| ☐      | 20  | Valid Parentheses              | Easy       | Stack   | ⭐⭐⭐          | 入门必做  |
| ☐      | 155 | Min Stack                      | Easy       | Stack设计 | ⭐⭐⭐          | 高频    |
| ☐      | 232 | Implement Queue using Stacks   | Easy       | Stack模拟 | ⭐⭐           | 理解结构  |
| ☐      | 394 | Decode String                  | Medium     | Stack   | ⭐⭐⭐          | 高频    |
| ☐      | 739 | Daily Temperatures             | Medium     | 单调栈     | ⭐⭐⭐⭐         | 必背    |
| ☐      | 496 | Next Greater Element I         | Easy       | 单调栈     | ⭐⭐⭐          | 模板    |
| ☐      | 503 | Next Greater Element II        | Medium     | 单调栈     | ⭐⭐⭐⭐         | 环形    |
| ☐      | 84  | Largest Rectangle in Histogram | Hard       | 单调栈     | ⭐⭐⭐⭐⭐        | 面试杀手  |
| ☐      | 42  | Trapping Rain Water            | Hard       | 单调栈/双指针 | ⭐⭐⭐⭐⭐        | 高频    |


## queue 
| Status | #   | Problem                           | Difficulty | Pattern | ⭐ Confidence | Notes |
| ------ | --- | --------------------------------- | ---------- | ------- | ------------ | ----- |
| ☐      | 933 | Number of Recent Calls            | Easy       | Queue   | ⭐⭐           | 入门    |
| ☐      | 225 | Implement Stack using Queues      | Easy       | Queue模拟 | ⭐⭐           |       |
| ☐      | 102 | Binary Tree Level Order Traversal | Medium     | BFS     | ⭐⭐⭐⭐         | 必背    |
| ☐      | 200 | Number of Islands                 | Medium     | BFS/DFS | ⭐⭐⭐⭐         | 高频    |
| ☐      | 994 | Rotting Oranges                   | Medium     | BFS     | ⭐⭐⭐⭐         | 模板题   |
| ☐      | 127 | Word Ladder                       | Hard       | BFS     | ⭐⭐⭐⭐⭐        | 面试经典  |
| ☐      | 239 | Sliding Window Maximum            | Hard       | 单调队列    | ⭐⭐⭐⭐⭐        | 必背    |
| ☐      | 542 | 01 Matrix                         | Medium     | BFS     | ⭐⭐⭐⭐         | 多源BFS |
| ☐      | 286 | Walls and Gates                   | Medium     | BFS     | ⭐⭐⭐⭐         |       |


 




---- 
## Array
| Status | #   | Problem                         | Difficulty | Pattern         | ⭐ Confidence | Notes                  |
| ------ | --- | ------------------------------- | ---------- | --------------- | ------------ | ---------------------- |
| ✅      | 1   | Two Sum                        | Easy       | HashMap / Array | ⭐            |                        |
| ✅      | 167 | Two Sum II                     | Easy       | Two Pointers    | ⭐            | The order is important |
| ☐      | 88  | Merge Sorted Array              | Easy       | Two Pointers    | ⭐            |                        |
| ☐      | 53  | Maximum Subarray                | Medium     | DP / Kadane     | ⭐            |                        |
| ☐      | 121 | Best Time to Buy and Sell Stock | Easy       | Greedy / Array  | ⭐            |                        |


---

## ListNode (Linked List)
** think about if need the dummy node** 
| Status | # | Problem | Difficulty | Pattern | ⭐ Confidence | Notes |
|--------|---|--------|-----------|---------|--------------|------|
| ✅  | 206 | Reverse Linked List | Easy | Reverse List / Iteration | ⭐ | |
| X | 21 | Merge Two Sorted Lists | Easy | Merge / Two Pointers | ⭐⭐ | dummy = Listnode(0) and the tail of the dummy listnode needs to be setup and move (tail = tail.next), after comparison between the two nodes |
| ✅ | 141 | Linked List Cycle | Easy | Fast and Slow Pointers | ⭐⭐ | iteration ensure fast and fast.next is not None, set slow =head , fast = head, only find if there is cycle|
| X | 142 | Linked List Cycle | Medium | Fast and Slow Pointers | ⭐⭐ | iteration ensure fast and fast.next is not None, set slow =head , fast = head| 
| X | 19 | Remove Nth Node From End of List | Medium | Fast and Slow Pointers | ⭐⭐ | Using the mapping to find the previous one before the removed one// using the fast pointer moved first n steps, and until it reches to the end. Need to set the dummy nodes. |
| ✅  | 876 | Middle of the Linked List | Easy | Fast and Slow Pointers | ⭐ | |
| ✅ | 83 | Remove Duplicates from Sorted List | Easy | Pointer Update | ⭐ | The list has been sorted |


---

## HashMap / Set
| Status | # | Problem | Difficulty | Pattern | ⭐ Confidence | Notes |
|--------|---|--------|-----------|---------|--------------|------|
| ✅ |  |  |  |  | ⭐⭐ | |
| ☐ |  |  |  |  | ⭐ | |
| ☐ |  |  |  |  | ⭐ | |




---

## Prefix Sum
| Status | # | Problem | Difficulty | Pattern | ⭐ Confidence | Notes |
|--------|---|--------|-----------|---------|--------------|------|
| ☐ |  |  | Prefix Sum | ⭐ | |
| ☐ |  |  | Subarray Sum | ⭐ | |



---



## Two Pointers
| Status | # | Problem | Difficulty | Pattern | ⭐ Confidence | Notes |
|--------|---|--------|-----------|---------|--------------|------|
| ☐ |  |  | Opposite Direction | ⭐ | |
| ☐ |  |  | Same Direction (Fast/Slow) | ⭐ | |



---

## Sliding Window
| Status | # | Problem | Difficulty | Pattern | ⭐ Confidence | Notes |
|--------|---|--------|-----------|---------|--------------|------|
| ☐ |  |  | Fixed Window | ⭐ | |
| ☐ |  |  | Variable Window | ⭐ | |



---

## Stack / Monotonic Stack
| Status | # | Problem | Difficulty | Pattern | ⭐ Confidence | Notes |
|--------|---|--------|-----------|---------|--------------|------|
| ☐ |  |  | Stack | ⭐ | |
| ☐ |  |  | Monotonic Stack | ⭐ | |

---

## Queue / Monotonic Queue
| Status | # | Problem | Difficulty | Pattern | ⭐ Confidence | Notes |
|--------|---|--------|-----------|---------|--------------|------|
| ☐ |  |  | Queue | ⭐ | |
| ☐ |  |  | Monotonic Queue | ⭐ | |

---

## Binary Search
| Status | # | Problem | Difficulty | Pattern | ⭐ Confidence | Notes |
|--------|---|--------|-----------|---------|--------------|------|
| ☐ |  |  | Basic Binary Search | ⭐ | |
| ☐ |  |  | Left / Right Bound | ⭐ | |

---

## Heap (Priority Queue)
| Status | # | Problem | Difficulty | Pattern | ⭐ Confidence | Notes |
|--------|---|--------|-----------|---------|--------------|------|
| ☐ |  |  | Min Heap | ⭐ | |
| ☐ |  |  | Top K | ⭐ | |

---

## Greedy
| Status | # | Problem | Difficulty | Pattern | ⭐ Confidence | Notes |
|--------|---|--------|-----------|---------|--------------|------|
| ☐ |  |  | Greedy | ⭐ | |

---

## Backtracking
| Status | # | Problem | Difficulty | Pattern | ⭐ Confidence | Notes |
|--------|---|--------|-----------|---------|--------------|------|
| ☐ |  |  | Subset / Permutation | ⭐ | |

---

## Graph (BFS / DFS)
| Status | # | Problem | Difficulty | Pattern | ⭐ Confidence | Notes |
|--------|---|--------|-----------|---------|--------------|------|
| ☐ |  |  | BFS | ⭐ | |
| ☐ |  |  | DFS | ⭐ | |

---

## Dynamic Programming
| Status | # | Problem | Difficulty | Pattern | ⭐ Confidence | Notes |
|--------|---|--------|-----------|---------|--------------|------|
| ☐ |  |  | 1D DP | ⭐ | |
| ☐ |  |  | 2D DP | ⭐ | |

---
