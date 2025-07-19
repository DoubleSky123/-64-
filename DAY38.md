# DAY 38｜322. Coin Change 279. Perfect Squares 139. Word Break
## 学习内容
[背包理论基础](https://programmercarl.com/%E8%83%8C%E5%8C%85%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%8001%E8%83%8C%E5%8C%85-1.html)
- dp数组含义：二维数组为 dp[i][j]：i表示物品，j表示背包容量。
- dp[i][j] 表示从下标为`[0-i]`的物品里任意取，放进容量为j的背包，价值总和最大是多少。
- 有两种情况推导出dp[i][j]：放物品i，不放物品i：
  - 不放物品i：背包容量为j，里面不放物品i的最大价值是`dp[i - 1][j]`
  - 放物品i：背包空出物品i的容量后，背包容量为`j - weight[i]`，`dp[i - 1][j - weight[i]]` 为背包容量为j - weight[i]且不放物品i的最大价值，那么`dp[i - 1][j - weight[i]] + value[i]` （物品i的价值），就是背包放物品i得到的最大价值。
- 递推公式： dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - weight[i]] + value[i])
- 初始化：dp[0][j]，即：i为0，存放编号0的物品的时候，各个容量的背包所能存放的最大价值。当 j < weight[0]的时候，dp[0][j] 应该是 0，因为背包容量比编号0的物品重量还小。当j >= weight[0]时，dp[0][j] 应该是value[0]，因为背包容量放足够放编号0物品。
- 遍历顺序：先遍历物品再遍历背包这个顺序更好理解。
## 322. Coin Change
[Leetcode Link](https://leetcode.cn/problems/coin-change/description/)-Medium
### Description
>You are given an integer array coins representing coins of different denominations and an integer amount representing a total amount of money.
>Return the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1.
>You may assume that you have an infinite number of each kind of coin.
>>**Example 1:**
>>**Input:**
>>coins = [1,2,5], amount = 11
>>**Output:**
>>3
>>**Explanation:**
>>11 = 5 + 5 + 1
### Code
#### Method 1 一维背包
>求装满背包的最少物品数量，dp[j] = min(dp[j-i]+1,dp[j])
```python
class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        '''
        dp[j]表示j个重量最少放几个钱
        i= 1
        j=1,1,2,3,4,5,6...
        i=2
        j=1,2,
        '''
        dp = [float('inf')]*(amount+1)
        dp[0] = 0
        for i in coins:
            for j in range(i,amount+1):
                dp[j] = min(dp[j-i]+1,dp[j])

        return dp[amount] if dp[amount] != float('inf') else -1
```
> - Time: O(Amount∗Sum(Coins))
> - Space: O(Amount)
#### Method 2 二维背包
```python

```
> - Time: O(N*Target)
> - Space: O(N*Target)
## 279. Perfect Squares
[Leetcode Link](https://leetcode.cn/problems/perfect-squares/description/)-Medium
### Description
>Given an integer n, return the least number of perfect square numbers that sum to n.
>A perfect square is an integer that is the square of an integer; in other words, it is the product of some integer with itself. For example, 1, 4, 9, and 16 are perfect squares while 3 and 11 are not.
>>**Example 1:**
>>**Input:**
>>n = 12
>>**Output:**
>>3
>>**Explanation:**
>>12 = 4 + 4 + 4.
### Code
#### Method 1 一维背包
> 求装满背包最少的物品数，dp[j] = min(dp[j-i*i]+1,dp[j])，完全平方数i*i，遍历背包的时候n**0.5。
```python
class Solution:
    def numSquares(self, n: int) -> int:
        dp = [float('inf')]*(n+1)
        dp[0] = 0
        for i in range(1,int(n**0.5)+1):
            for j in range(i*i,n+1):
                dp[j] = min(dp[j-i*i]+1,dp[j])

        return dp[n]
```
> - Time: O(N∗Sqrt(N))
> - Space: O(N)
#### Method 2 二维背包
```python

```
> - Time: O(N*Target)
> - Space: O(N*Target)
## 139. Word Break
[Leetcode Link](https://leetcode.cn/problems/word-break/description/)-Medium
### Description
>Given a string s and a dictionary of strings wordDict, return true if s can be segmented into a space-separated `sequence` of one or more dictionary words.
>Note that the same word in the dictionary may be reused multiple times in the segmentation.
>>**Example 1:**
>>**Input:**
>>s = "leetcode", wordDict = ["leet","code"]
>>**Output:**
>>true
>>**Explanation:**
>>Return true because "leetcode" can be segmented as "leet code".
### Code
#### Method 1 一维背包
> 因为要求string有序，所以是求排列，先遍历背包，再遍历物品。背包是string长度，dp[j]是当装满j的时候单词是否在word dict中，返回true/false。
> dp[j]为不放单词i的时候，j-i是否为true，如果为true，判断i是否也为true，如果是，dp[j]为true。
```python
class Solution:
    def wordBreak(self, s: str, wordDict: List[str]) -> bool:
        size = len(s)
        dp = [False]* (size+1)

        dp[0] = True
        
        ## wordDict有序，所以是求排列
        for j in range(1,size+1): ##背包
            for i in range(j):    ##单词
                if dp[i] is True and s[i:j] in wordDict:
                    dp[j] = True 
                    break
                    
        return dp[size]
```
> - Time: O(N^2)
> - Space: O(N)
#### Method 2 二维背包
```python

```
> - Time: O(N*Target)
> - Space: O(N*Target)
## 今日心得
- 前两题是求装满完全背包需要的最少物品数，最后一题的递推公式比较难想，而且需要理解题意是求排列。
