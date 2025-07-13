# DAY 32｜509. Fibonacci Number 70. Climbing Stairs 746. Min Cost Climbing Stairs
## 学习内容
[动态规划理论基础](https://programmercarl.com/%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)
- 动规五步曲：
    - 确定dp数组（dp table）以及下标的含义
    - 确定递推公式
    - dp数组如何初始化
    - 确定遍历顺序
    - 举例推导dp数组
## 509. Fibonacci Number
[Leetcode Link](https://leetcode.cn/problems/fibonacci-number/description/)-Easy
### Description
>The Fibonacci numbers, commonly denoted F(n) form a sequence, called the Fibonacci sequence, such that each number is the sum of the two preceding ones, starting from 0 and 1. That is,
>F(0) = 0, F(1) = 1 F(n) = F(n - 1) + F(n - 2), for n > 1. Given n, calculate F(n).
>>**Example 1:**
>>**Input:**
>>n = 2
>>**Output:**
>>2
>>**Explanation:**
>>F(2) = F(1) + F(0) = 1 + 0 = 1.
### Code
>dp[i] = dp[i] = dp[i-1] + dp[i-2]
>dp[0] = 0 dp[1] = 1
```python
class Solution:
    def fib(self, n: int) -> int:
        if n == 0:
            return 0
        dp = [0] * (n+1)
        dp[0] = 0
        dp[1] = 1

        for i in range(2,n+1):
            dp[i] = dp[i-1] + dp[i-2]
        
        return dp[n]
```
> - Time: O(N)
> - Space: O(N)
## 70. Climbing Stairs
[Leetcode Link](https://leetcode.cn/problems/climbing-stairs/description/)-Easy
### Description
>You are climbing a staircase. It takes n steps to reach the top.Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?
>>**Example 1:**
>>**Input:**
>>n = 2
>>**Output:**
>>2
>>**Explanation:**
>>There are two ways to climb to the top. 1. 1 step + 1 step 2. 2 steps
### Code
>dp[i] = dp[i-1] + dp[i-2]
>dp[1] = 1 dp[2] = 2.注意这里不能初始化dp[0]因为没有意义，从0开始会报错。
```python
class Solution:
    def climbStairs(self, n: int) -> int:
        '''
        dp[i] = have dp[i] ways to get n steps
        i = i steps
        '''
        if n <=1:
            return n
        dp = [0] * (n+1)
        dp[1] = 1
        dp[2] = 2
        # dp[3] = 3
        # dp[4] = 5

        for i in range(3,n+1):
            dp[i] = dp[i-1] + dp[i-2]
        
        return dp[n]
```
> - Time: O(N)
> - Space: O(N)
## 746. Min Cost Climbing Stairs
[Leetcode Link](https://leetcode.cn/problems/min-cost-climbing-stairs/description/)-Easy
### Description
>You are given an integer array cost where cost[i] is the cost of ith step on a staircase. Once you pay the cost, you can either climb one or two steps.
>You can either start from the step with index 0, or the step with index 1. Return the minimum cost to reach the top of the floor.
>>**Example 1:**
>>**Input:**
>>cost = [10,15,20]
>>**Output:**
>>15
>>**Explanation:**
>>You will start at index 1. Pay 15 and climb two steps to reach the top. The total cost is 15.
### Code
>dp[i] = min(dp[i-1]+cost[i-1],dp[i-2]+cost[i-2])
>dp[0] = 0 dp[1] = 0.题意到达top是在len+1的位置，在数组以外，并且从当前台阶往上跳，才会花费当前台阶的cost。
```python
class Solution:
    def minCostClimbingStairs(self, cost: List[int]) -> int:
        '''
        dp[i] = get i step pay min(dp[i]) cost
        i = ith step
        top = len()+1
        '''
        dp = [0] * (len(cost)+1)
        dp[0] = 0
        dp[1] = 0

        for i in range(2,len(cost)+1):
            dp[i] = min(dp[i-1]+cost[i-1],dp[i-2]+cost[i-2])
        print(dp)

        return dp[-1]
```
> - Time: O(N)
> - Space: O(N)
## 今日心得
- 动规开始，牢记动规五步曲。基础题目尤其要注意初始化如何做和确定递推公式，容易出错。
