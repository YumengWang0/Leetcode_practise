
--- 

## queue : from collections import deque 
q = deque() 
1. pop  q.popleft() 
2. push  q.append()


#### solve problem 
1. BFS 
2. Find the shortest path 
3. Layer process 
4. slicing window 




#### how to utilize the stack for queue realizing:  two stack 


class myqueue: 

    def __init__(self,): 
        self.in_stack = []
        self.out_stack = [ ]

    def push(self, x):

        self.in_stack.append(x)

      
    def pop(self): 

        if not self.out_stack:
            while self.in_stack:
                self.out_stack.append( self.in_stack.pop())

        return self.out_stack.pop()

    def peek(self): 
        if not self.out_stack:
                    while self.in_stack:
                        self.out_stack.append( self.in_stack.pop())


        return self.out_stack[-1]


    def empty(self):

        return len(self.in_stack) == 0 and len(self.out_stack) == 0 





        



 Exp : [3, 5, 7, 8] if pop : pop 3 first. top : 3, push append 
[8, 7, 5, 3] then pop will be the queue 







#### code 

BFS 模版

def  bfs( start): 
    #put the start into the queue 
    q = queue([start])


    #visited 
    visited = set( [start])

    while q: 
        cur = pop.left()
        for nei in neighbour(cur): 
            if nei not in visited: 
                visited.add(nei)
                q.append(nei)




from collections import deque

def maxSlidingWindow(nums, k):
    q = deque()
    res = []

    for i in range(len(nums)):
        while q and nums[q[-1]] < nums[i]:
            q.pop()

        q.append(i)

        if q[0] == i - k:
            q.popleft()

        if i >= k - 1:
            res.append(nums[q[0]])

    return res




