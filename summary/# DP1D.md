# dynamic programing 一种重复调用递归

with the stored table, the recurrent algorithm don't need to recompute the quantities. 
Thus use a array to store the result in the middle. 
The recurrent algorithm is usually decomposed from the top to bottom. 
But the DP is from bottom to top. 

- Keys: 

    - define the repeated computed parts to find the dp equation 
        - dp[i] 表示到第i个位置的最优解。 每一个i 位置都是当前的最优解。以i位置为结尾的从前往后或者从后往前

    - Initialize the states and the boundary
    - Be careful about the size. usually is set (n + 1) 


    - Usually 1D dont need to initialize a （n+1）size array。 length of 2 would be good 

- 1D problem list 


1. 线性递推: Fibonacci, Climbing stairs
2. 选择/不选: House Robber// Partition problems

 - dp[i] = max( dp[i-1], # skip
                dp[i-2] + nums[i] # take the current 
                )

3. 子序列计数: Decode Ways. Distinct Subsequence, LIS变种

 - dp[i][j] = using s[0:i] to match t[0:j]
 dp[j] += dp[j-1]


4. 区间累积 (票价 Minimum ticket cost) 

 - dp[i] = min cost up to i 


# Wrong:
1. 509 : when n<=1;
        iteration should until n. for i in range(2, n+1)

70. Yes
198 Yes .But this initialize with shape of n .


213 “线性选 or 不选问题”变成“两个线性问题” 

509， 70，
198， 213， 
740 Delete and Earn
2320 Maximum Number of Ways to Pick Elements  
91， 639，
115， 940，
392 Is Subsequence
72 Edit Distance
940 Distinct Subsequences II 

983， 467 
