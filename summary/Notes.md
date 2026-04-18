## Notes of Algorithm and Data Structures

---

## 0421
Heap problems, radix sort, and sorting algorithms  

---

## 0420
Heap and heap sorting, hash table, ordered array  

---

## 0419
Randomized quick sort, randomized selection algorithm  

---

## 0418 Lesson Notes: Sorting

#### Merge Sorting

1. Idea: left side sorting + right side sorting → merge the sorted left and right  
2. Merge process: compare and copy the smaller one until one side is used, then append the rest (two pointers approach)  

3. Two methods  

-- **recursive (top-down)**  
-- divide into left and right until size = 1, then merge back  
-- code:  

-- **iterative (bottom-up)**  
-- use step size (subarray size)  
-- start from step = 1, merge every two subarrays  
-- every 2 * step is ordered after merge  
-- then increase step = step * 2, repeat merge  
-- code:  

4. time complexity: O(n log n) > O(n^2)  
5. space complexity: O(n), merge step needs extra space (not in-place)  
6. stable sorting  
7. comparisons during merge are not wasted, they directly contribute to ordered result  
8. divide and conquer  

---

##### Divide and Conquer

1. Idea: if a problem can be divided into left and right subproblems,  
   and also needs to handle the crossing part between left and right  

   example: find the sum of numbers on the left that are smaller than the current number  

2. Left crossing right:  
-- after left and right are sorted, the crossing part can be handled efficiently  
-- thus, ensure left and right are sorted before handling crossing  

Example: left min sum  
LeetCode 493  

---