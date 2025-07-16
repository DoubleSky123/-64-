# DAY 34｜62. Unique Paths 63. Unique Paths II 343. Integer Break 96. Unique Binary Search Trees(Skip)
## 学习内容
[动态规划理论基础](https://programmercarl.com/%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)
- 动规五步曲：
    - 确定dp数组（dp table）以及下标的含义
    - 确定递推公式
    - dp数组如何初始化
    - 确定遍历顺序
    - 举例推导dp数组
## 62. Unique Paths
[Leetcode Link](https://leetcode.cn/problems/unique-paths/description/)-Medium
### Description
>There is a robot on an m x n grid. The robot is initially located at the top-left corner (i.e., grid[0][0]).
>The robot tries to move to the bottom-right corner (i.e., grid[m - 1][n - 1]). The robot can only move either down or right at any point in time.
>Given the two integers m and n, return the number of possible unique paths that the robot can take to reach the bottom-right corner.
>The test cases are generated so that the answer will be less than or equal to 2 * 109.
>>**Example 1:**
>>**Input:**
>>m = 3, n = 7
>>**Output:**
>>28
### Code
>二维dp数组，要明确题意只能往下或往右走一格，因此dp[i][j]由dp[i-1][j]+dp[i][j-1]得到。
>同时需要将第一行和第一列都进行初始化为1，不然得不到其他格子的dp值。
```python
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        '''
        dp[i][j] = how many ways to the [i][j]position
        '''
        dp = [[0] * n for _ in range(m)]
        ###每一格都要初始化
        for i in range(m):
            dp[i][0] = 1
        for j in range(n):
            dp[0][j] = 1
            
        for i in range(1,m):
            for j in range(1,n):
                dp[i][j] = dp[i-1][j] + dp[i][j-1]
                print(dp)
        return dp[-1][-1]
```
> - Time: O(MN)
> - Space: O(MN)
## 63. Unique Paths II
[Leetcode Link](https://leetcode.cn/problems/unique-paths-ii/description/)-Medium
### Description
>You are given an m x n integer array grid. There is a robot initially located at the top-left corner (i.e., grid[0][0]).
>The robot tries to move to the bottom-right corner (i.e., grid[m - 1][n - 1]). The robot can only move either down or right at any point in time.
>An obstacle and space are marked as 1 or 0 respectively in grid. A path that the robot takes cannot include any square that is an obstacle.
>Return the number of possible unique paths that the robot can take to reach the bottom-right corner.The testcases are generated so that the answer will be less than or equal to 2 * 109.
>>**Example 1:**
>>**Input:**
>>obstacleGrid = [[0,0,0],[0,1,0],[0,0,0]]
>>**Output:**
>>2
>>**Explanation:**
>>There is one obstacle in the middle of the 3x3 grid above. There are two ways to reach the bottom-right corner: 1. Right -> Right -> Down -> Down 2. Down -> Down -> Right -> Right
### Code
>再上一题基础上判断障碍是否为0，注意起点和终点如果有障碍直接return 0.初始化第一行和第一列如果碰到障碍，后面的格子都为0，因为走不过去了。
```python
class Solution:
    def uniquePathsWithObstacles(self, obstacleGrid: List[List[int]]) -> int:
        if obstacleGrid[0][0] == 1 or obstacleGrid[-1][-1] == 1:
            return 0
        n = len(obstacleGrid[0])
        m = len(obstacleGrid)

        dp = [[0]*n for _ in range(m)]

        for i in range(m):
            if obstacleGrid[i][0] == 1:
                break
            dp[i][0] = 1
        for j in range(n):
            if obstacleGrid[0][j] == 1:
                break
            dp[0][j] = 1
        
        for i in range(1,m):
            for j in range(1,n):
                if obstacleGrid[i][j] == 0:
                    dp[i][j] = dp[i-1][j]+dp[i][j-1]
        return dp[-1][-1]
```
> - Time: O(MN)
> - Space: O(MN)
## 343. Integer Break
[Leetcode Link](https://leetcode.cn/problems/integer-break/description/)-Medium
### Description
>Given an integer n, break it into the sum of k positive integers, where k >= 2, and maximize the product of those integers.Return the maximum product you can get.
>>**Example 1:**
>>**Input:**
>>n = 2
>>**Output:**
>>1
>>**Explanation:**
>>2 = 1 + 1, 1 × 1 = 1.
### Code
>明确3种情况取最大值：1. 拆成2个数 j*(i-j)，2.拆成3个及以上的数为j * dp[i-j], dp[i-j]为除j外上一个被拆数的最大乘积，还有dp[i].
>初始化从2开始。
```python
class Solution:
    def integerBreak(self, n: int) -> int:
        '''
        dp[i] = break i and get the max product dp[i]
        1. break to 2 nums j*(i-j)
        2. break to 3 or more nums j*dp[i-j]
        '''
        dp = [0]*(n+1)
        dp[2] = 1

        for i in range(3,n+1):
            for j in range(1,i//2+1):
                dp[i] = max(j*(i-j),j*dp[i-j],dp[i])
        return dp[n]
```
> - Time: O(N^2)
> - Space: O(N)
## 96. Unique Binary Search Trees(Skip)
[Leetcode Link](https://leetcode.cn/problems/unique-binary-search-trees/description/)-Medium
## 今日心得
- 今天的dp题目已经逐渐变难，体会了二维dp数组，先跳过一题后面再补。
