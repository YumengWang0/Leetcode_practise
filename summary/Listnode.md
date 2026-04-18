 # Linked List 高频面试题总结（模板版）

---

## ✅ 206. Reverse Linked List

* **考点**: 指针操作 / 迭代反转
* **核心思路**:

  * 使用三个指针：`prev`, `cur`,  the `temp` is for stroing 
  * 每次反转当前节点指向
  * 最终 `prev` 为新头节点

```python
prev = None
cur = head

while cur:
    nxt = cur.next
    cur.next = prev
    prev = cur
    cur = nxt

return prev
```

* **注意**:

  * 一定要先存 `next`
  * 返回 `prev`
* **单调性**: ❌
* **是否需要排序**: ❌

---

## ✅ 21. Merge Two Sorted Lists

* **考点**: 双指针 / dummy 技巧
* **核心思路**:

  * 用 dummy + tail 构建新链表
  * 每次接较小节点

```python
dummy = ListNode(0)
tail = dummy

while l1 and l2:
    if l1.val <= l2.val:
        tail.next = l1
        l1 = l1.next
    else:
        tail.next = l2
        l2 = l2.next
    tail = tail.next

if l1:
    tail.next = l1
else:
    tail.next = l2

return dummy.next
```

* **注意**:

  * 接节点，不新建
  * 处理剩余链表
* **单调性**: ✅
* **是否需要排序**: ❌（已排序）

---

## ✅ 141. Linked List Cycle

* **考点**: 快慢指针
* **核心思路**:

  * slow 走 1 步，fast 走 2 步
  * 相遇 → 有环

```python
slow = fast = head

while fast and fast.next:
    slow = slow.next
    fast = fast.next.next
    if slow == fast:
        return True

return False
```

* **注意**:

  * 判空条件
* **单调性**: ✅
* **是否需要排序**: ❌

---

## ✅ 83. Remove Duplicates from Sorted List

* **考点**: 指针更新
* **核心思路**:

  * 如果相同 → 跳过

```python
cur = head

while cur and cur.next:
    if cur.val == cur.next.val:
        cur.next = cur.next.next
    else:
        cur = cur.next

return head
```

* **注意**:

  * 必须是有序链表
* **单调性**: ✅
* **是否需要排序**: ❌

---

## ✅ 19. Remove Nth Node From End of List

* **考点**: 快慢指针（gap）
* **核心思路**:

  * fast 先走 n 步
  * 同时移动
  * slow 停在删除节点前

```python
dummy = ListNode(0)
dummy.next = head

slow = fast = dummy

for _ in range(n):
    fast = fast.next

while fast.next:
    slow = slow.next
    fast = fast.next

slow.next = slow.next.next

return dummy.next
```

* **注意**:

  * dummy 很关键（删除头节点）
* **单调性**: ✅
* **是否需要排序**: ❌

---

## ✅ 876. Middle of the Linked List

* **考点**: 快慢指针
* **核心思路**:

  * fast 两步，slow 一步
  * fast 到尾 → slow 在中点

```python
slow = fast = head

while fast and fast.next:
    slow = slow.next
    fast = fast.next.next

return slow
```

* **注意**:

  * 偶数返回第二个中点
* **单调性**: ✅
* **是否需要排序**: ❌

 

# 🔥 模式总结

当你只是想“从头开始看”，而且 不需要以后再单独用 head 的时候，  not set curr = head 
other situation: 
保留原来的 head, 但又需要一个指针往后走(遍历 / 查找 / 计数) , set curr = head, curr for the move and head can save the result 

什么时候必须用 dummy，不能只靠 head？
当题目里可能, 删除头节点，或者你要 新拼接一条链表，就非常推荐用 dummy。
dummy = ListNode(0)
dummy.next = head
return dummy.next



## 1️⃣ 反转类

* 206
  👉 模板：`prev / cur / next`

---

## 2️⃣ 合并类

* 21
  👉 模板：`dummy + tail`
---


## 3️⃣ 快慢指针类（最重要）

* 141（判环）
* 19（删除倒数）
* 876（找中点）

 