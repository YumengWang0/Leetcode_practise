 
## Stack 实现了将递归 变为更简单的迭代方法
1.push 
2.pop 
3.peek 

#### usually to solve 
- 括号匹配（） 
- monotonic stack (compare the small and larger ), finding the next greater element , largest rectangale 
- inverse order ， 因为stack就是去反转顺序的
- computation (3 + 5)/ 2. use two stacks 
- DFS not recurssive 
code:
    stack = [root]
    while stack: 
        node = stack.pop()
- 最近元素 






#### How to use one queue for the stack


from collections import deque 
class mystack: 

    def __init__(self):

        self.q = deque()


    def push(self, x): 
        self.q. append( x)

        # use the queue operation to realize the stack 
        for _ in range( len(self.q) -1 ):  #除去当前元素，thus -1 
            out = self.q. popleft()
            self.q.append( out) 
        
    def pop(self): # 队头 = 栈顶 
        return self.q.popleft()

    def top(self):
        return self.q[0] 

    def empty(self):
        retrun len(self.q) == 0


Exp: num_arr = [3, 5, 7, 8]. push each element. when pop it will out 8 first 
    push 3 : [3]
    push 5 : len(self.q) =2 , self.q= [3, 5], 把 3 popleft，append 进去 [5, 3, ]
    push 7: len(self.q) =3 , self.q= [ 5，3 ], first pop 5, [3, 7, 5], pop 3 then [7, 5, 3]

    pop the left in the queue 

#### code 

- Monotonic stack : finding the right side larger 

def monotonic_stack(nums):
    # define the stack store with the index ,find the index that finds the next larger element 

    # 
    res = [-1] * len(nums)

    for i in range( len(nums)): 

        while stack and nums[i]> nums[stack[-1]]: 
            # the current is larger than the stack[-1]
            idx = stack.pop()
            res[idx] = nums[i] 

        stack.append(i)

    return res 


- Monotonic stack: finding the right side smaller 
     while stack and nums[i]> nums[stack[-1]] changes to stack and nums[i]<  nums[stack[-1]] 




- Other styles 




