# DAY 37｜518. Coin Change II 377. Combination Sum IV
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
## 518. Coin Change II
[Leetcode Link](https://leetcode.cn/problems/coin-change-ii/description/)-Medium
### Description
>You are given an integer array coins representing coins of different denominations and an integer amount representing a total amount of money.
>Return the number of combinations that make up that amount. If that amount of money cannot be made up by any combination of the coins, return 0.
>You may assume that you have an infinite number of each kind of coin. The answer is guaranteed to fit into a signed 32-bit integer.
>>**Example 1:**
>>**Input:**
>>amount = 5, coins = [1,2,5]
>>**Output:**
>>4
>>**Explanation:**
>>there are four ways to make up the amount: 5=5 5=2+2+1 5=2+1+1+1 5=1+1+1+1+1
### Code
#### Method 1 一维背包
>完全背包遍历背包的时候正序遍历即可，倒序每个物品只会添加一遍，正序每个物品会添加多遍，就是完全背包。
>这道题就是问的是装满背包一共有多少种方法，所以dp[j] += dp[j-i]
```python
class Solution:
    def change(self, amount: int, coins: List[int]) -> int:
        dp = [0] * (amount+1)
        dp[0] = 1
        for i in coins:
            for j in range(i,amount+1):
                dp[j] += dp[j-i]
        return dp[amount]
```
> - Time: O(N*M)
> - Space: O(N)
#### Method 2 二维背包
```python

```
> - Time: O(N*Target)
> - Space: O(N*Target)
## 377. Combination Sum IV
[Leetcode Link](https://leetcode.cn/problems/combination-sum-iv/description/)-Medium
### Description
>Given an array of distinct integers nums and a target integer target, return the number of possible combinations that add up to target.
>The test cases are generated so that the answer can fit in a 32-bit integer.
>>**Example 1:**
>>**Input:**
>>nums = [1,2,3], target = 4
>>**Output:**
>>7
>>**Explanation:**
>>The possible combination ways are: (1, 1, 1, 1) (1, 1, 2) (1, 2, 1) (1, 3) (2, 1, 1) (2, 2) (3, 1) Note that different sequences are counted as different combinations.
### Code
#### Method 1 一维背包
> 这道题也是装满背包有多少种方法。但是不同的是，这道题求的是排列数，元素顺序的不同算作不同的方法，所以要先遍历背包再遍历物品。如果像上一题求组合数，就先遍历物品再遍历背包。
```python
class Solution:
    def combinationSum4(self, nums: List[int], target: int) -> int:
        dp = [0]*(target+1)

        dp[0] = 1
        for j in range(1,target+1):
            for i in nums:
                if j-i >=0:
                    dp[j] += dp[j-i]
        return dp[target]
```
> - Time: O(N*Target)
> - Space: O(Target)
#### Method 2 二维背包
```python

```
> - Time: O(N*Target)
> - Space: O(N*Target)
## 今日心得
- 今天学习了完全背包，完全背包的二层遍历顺序正好和01背包相反，但是需要注意：求`组合数`就是先遍历物品再遍历背包，求`排列数`就是先遍历背包再遍历物品。
