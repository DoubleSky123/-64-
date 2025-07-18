# DAY 36｜1049. Last Stone Weight II 494. Target Sum 474. Ones and Zeroes
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
## 1049. Last Stone Weight II
[Leetcode Link](https://leetcode.cn/problems/last-stone-weight-ii/description/)-Medium
### Description
>You are given an array of integers stones where stones[i] is the weight of the ith stone.
>We are playing a game with the stones. On each turn, we choose any two stones and smash them together. Suppose the stones have weights x and y with x <= y. The result of this smash is:
>If x == y, both stones are destroyed, and If x != y, the stone of weight x is destroyed, and the stone of weight y has new weight y - x. At the end of the game, there is at most one stone left.
>Return the smallest possible weight of the left stone. If there are no stones left, return 0.
>>**Example 1:**
>>**Input:**
>>stones = [2,7,4,1,8,1]
>>**Output:**
>>1
>>**Explanation:**
>>We can combine 2 and 4 to get 2, so the array converts to [2,7,1,8,1] then, we can combine 7 and 8 to get 1, so the array converts to [2,1,1,1] then,
>>we can combine 2 and 1 to get 1, so the array converts to [1,1,1] then, we can combine 1 and 1 to get 0, so the array converts to [1], then that's the optimal value.
### Code
#### Method 1 一维背包
> 当两组石头重量最接近的时候，相撞产生结果最小，因此抽象为背包问题是：背包重量就是数组和的一半。问装满背包的最大重量是多少，最后用sum-最大重量-最大重量。
> dp数组还是一维背包的dp数组：dp[j] = max(dp[j],dp[j-stones[i]]+stones[i])
```python
class Solution:
    def lastStoneWeightII(self, stones: List[int]) -> int:
        target = sum(stones)//2

        dp = [0]*(target + 1)

        for i in range(len(stones)):
            for j in range(target,stones[i]-1,-1):
                dp[j] = max(dp[j],dp[j-stones[i]]+stones[i])
        return sum(stones)-dp[target]-dp[target]
```
> - Time: O(N*Target)
> - Space: O(Target)
#### Method 2 二维背包
```python

```
> - Time: O(N*Target)
> - Space: O(N*Target)
## 494. Target Sum
[Leetcode Link](https://leetcode.cn/problems/target-sum/description/)-Medium
### Description
>You are given an integer array nums and an integer target.
>You want to build an expression out of nums by adding one of the symbols '+' and '-' before each integer in nums and then concatenate all the integers.
>For example, if nums = [2, 1], you can add a '+' before 2 and a '-' before 1 and concatenate them to build the expression "+2-1".
>Return the number of different expressions that you can build, which evaluates to target.
>>**Example 1:**
>>**Input:**
>>nums = [1,1,1,1,1], target = 3
>>**Output:**
>>5
>>**Explanation:**
>>There are 5 ways to assign symbols to make the sum of nums be target 3. -1 + 1 + 1 + 1 + 1 = 3 +1 - 1 + 1 + 1 + 1 = 3 +1 + 1 - 1 + 1 + 1 = 3 +1 + 1 + 1 - 1 + 1 = 3 +1 + 1 + 1 + 1 - 1 = 3
### Code
#### Method 1 一维背包
> left = 正数和，right = 负数和，left+right = target, left-right = sum -> 2left = target + sum, 背包重量即为(target + sum)//2，如果是奇数，说明无解。
> 抽象为背包问题即为，装满重量背包一共有多少种方法，dp数组为dp[j] += dp[j-i]。dp[j] 为装满重量为j的背包一共有dp[j]种方法，不放i这个数的时候，就有dp[j-i]+1种方法。
> j = 5, i = 1, 已经有dp[4]种方法，再放一个i，就是dp[4]+1.初始化dp[0] = 1.
```python
class Solution:
    def findTargetSumWays(self, nums: List[int], target: int) -> int:
        total_sum = sum(nums)
        if abs(target) > total_sum:
            return 0
        if (target+total_sum) %2 == 1:
            return 0
        target_sum = (target + total_sum) // 2
        dp = [0] * (target_sum+1)
        dp[0] = 1
        for i in nums:
            for j in range(target_sum,i-1,-1):
                dp[j] += dp[j-i]
        return dp[target_sum]
```
> - Time: O(N*Target)
> - Space: O(Target)
#### Method 2 二维背包
```python

```
> - Time: O(N*Target)
> - Space: O(N*Target)
## 474. Ones and Zeroes
[Leetcode Link](https://leetcode.cn/problems/ones-and-zeroes/description/)-Medium
### Description
>You are given an array of binary strings strs and two integers m and n.
>Return the size of the largest subset of strs such that there are at most m 0's and n 1's in the subset.
>A set x is a subset of a set y if all elements of x are also elements of y.
>>**Example 1:**
>>**Input:**
>>strs = ["10","0001","111001","1","0"], m = 5, n = 3
>>**Output:**
>>4
>>**Explanation:**
>>The largest subset with at most 5 0's and 3 1's is {"10", "0001", "1", "0"}, so the answer is 4.
>>Other valid but smaller subsets include {"0001", "1"} and {"10", "1", "0"}. {"111001"} is an invalid subset because it contains 4 1's, greater than the maximum of 3.
### Code
#### Method 1 一维背包
>有重量为m和n的背包，需要分别装满0和1.物品重量就是每个子string里含有x个0，y个1，价值就是个数1.
>dp[i][j]表示有i个0，j个1的背包含有的最大字符串个数为dp[i][j]，dp数组：dp[i][j] = max(dp[i-x][j-y]+1,dp[i][j]).
```python
class Solution:
    def findMaxForm(self, strs: List[str], m: int, n: int) -> int:
        dp = [[0]*(n+1) for _ in range(m+1)]

        for s in strs:
            zeroNum = s.count('0')
            oneNum = len(s)-zeroNum
            for i in range(m,zeroNum-1,-1):
                for j in range(n,oneNum-1,-1):
                    dp[i][j] = max(dp[i][j],dp[i-zeroNum][j-oneNum]+1)
        return dp[m][n]
```
> - Time: O(MN*K)
> - Space: O(MN)
## 今日心得
- 三道不同的01背包应用，多非常难，还要多练习。1. 装满背包重量为j的物品价值是多少，2.装满背包重量为j一共有多少种方法，3.装满背包重量为i，j的物品个数是多少
