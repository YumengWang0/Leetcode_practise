## 随机快速排序 Randomized quick sort

steps:
1. Random pick a pivot 
2.   < pivot 
     = pivot
     > pivot 


3. Recursive sort the left side and the right side 


complexity is worst O(N^2)
because this is random algorithm , the complexity average is O(Nlogn) 

- code 
def quicksort(nums):
    if len(nums) <=1 : 
        return nums 

    pivot = random.choice( nums)  # l + random.randn()* (r - l + 1 )
    left = []
    mid = []

    right = []

    for x in nums: 
        if x< pivot: 
            left.append( x)

        elif x == pivot: 
            mid.append( x)

        else: 
            right.append(x) 

    # quicksort reasult is the list, thus can add together.
    return quicksort(left) + mid +  quicksort(right)


荷兰国旗问题 










## Merge sort 
1. time complextity nlogn 
2. space complexity o(n)

- array
def merge_sort(nums): 
    if len(nums)<=1: 
        return nums

    mid = len(num)//2

    left =merge_sort(nums[:mid])
    right = merge_sort(nums[mid:])

    return merge(left, right)


def merge(left, right): 
    res = []

    # the pointer at left and right 
    i = j = 0 
    while i < len(left) and j <len(right): 
        if left[i]< right[j]:
            res.append(left[i])
            i+=1 

        else:
            res.append(right[j])
            j +=1 

    res.extend( left[i:])
    res.extend( right[j:])

    return res 


- Listnode 

def sortList(head): 
    if not head or not head.next:
        return head


    slow = head 
    fast = head.next 

    while fast and fast.next: 
        slow = slow.next
        fast = fast.next.next 


    mid = slow.next 
    slow.next = None 

    # split two array 

    left = sortList(head) 
    right = sortList(mid) 

    return merge(left, right)


def merge(left, right): 
    dummy = ListNode(0)
    tail = dummy 


    while left.next and right.next: 
        if left.val <right.val: 
            tail.next = left 
            left = left.next 

        else:
            tail.next = right 
            right =  right.next

        tail = tail.next 

    
    if left is not None: 
        tail.next = left 

    if right is not None: 
        tail.next = right 

    rturn dummhy.next 
    



912  21, 88 
148, 23 

53 
169 

315 
493
327 




------
## Divide and conquer 
1. If the problem can be left result, right result and the result crossing left to right 
2. If the sorting help when computing the crossing left to right 

### Recursive method. 
1. left, right result 
2. Return the ordered left and right 
3. Merge left right 

4.  def solve(): 

        if base_case: 
            reutrn result 

        left = solve( left_part)

        right = solve(right_part)

        return merge(left, right )

