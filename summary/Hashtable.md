# Hash / 有序表 / Heap 核心笔记

## 一、数组 vs Hash vs 有序表 vs Heap

### 1. 数组
特点： 连续存储， 下标访问快，  如果 key 范围固定，优先考虑数组

适用： 26 个字母， 0~100 分数， 范围小且固定的整数 key， if mapping key can be represented with index 

---

### 2. Hash 结构
HashSet and  HashMap ： usually with set and dict 

特点：平均增删查：`O(1)`，  不保证有序， 常数不小
---

### 3. 有序表
- TreeMap：按 key 自动排序的 map， 找最小/最大 key， 找 >= x 的最小 key
- TreeSet：自动排序的 set， 去重， 有序

特点：自动按大小有序， 增删查：`O(log N)`， 可做范围查询、找最小/最大、前驱/后继

底层常见实现： 红黑树， 一种平衡二叉搜索树， 保证增删查大约 `O(log N)`



 

---

### 4. Heap / Priority Queue
特点：只保证堆顶最小或最大， 不保证整体有序， 插入 / 弹出堆顶：`O(log N)`， 查看堆顶：`O(1)`

--- 

## 二、HashSet and HashMap

- HashSet： 只存值， 去重 ， 判断某元素是否存在。 add ， i in s， remove 

- HashMap 存 `key -> value`， keys(), values(), get


---

## 三、比较器 Comparator

- 所有对象都在内存中，某些自定义对象默认比较的不是“内容”，而更像“对象身份”

比较器决定：谁更小， 小在前， 除非改逻辑， 排序怎么排， 堆顶是谁， 有序表如何维护顺序

例子：按年龄升序，再按名字字典序
```python
people = [("Tom", 20), ("Alice", 18), ("Bob", 18)]
people.sort(key=lambda x: (x[1], x[0]))
```

- 字典序 = 像查字典一样逐字符比较。

 
## 四、Heap / Priority Queue

### Heap， 完全二叉树
- 小根堆：父节点 <= 子节点
- 大根堆：父节点 >= 子节点

### Priority Queue ，import heapq 
- 通常用堆实现， 每次弹出优先级最高的元素， 默认为小根堆。 hasppush， heappop can get 堆顶。 
- Heap 不是 set，所以可以重复插入：
