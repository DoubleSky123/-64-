# DAY 35｜416. Partition Equal Subset Sum
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
## 416. Partition Equal Subset Sum
[Leetcode Link](https://leetcode.cn/problems/partition-equal-subset-sum/description/)-Medium
### Description
>Given an integer array nums, return true if you can partition the array into two subsets such that the sum of the elements in both subsets is equal or false otherwise.
>>**Example 1:**
>>**Input:**
>>nums = [1,5,11,5]
>>**Output:**
>>true
>>**Explanation:**
>>The array can be partitioned as [1, 5, 5] and [11].
### Code
#### Method 1 二维背包
```python
class Solution:
    def canPartition(self, nums: List[int]) -> bool:
        '''
        二维背包
        '''
        if sum(nums)%2 == 1:
            return False
        target = int(sum(nums)/2)
        dp = [[0] * (target+1) for _ in range(len(nums))]
        for j in range(1,target + 1):
            if j >= nums[0]:
                dp[0][j] = nums[0]

        for i in range(1,len(nums)):
            for j in range(target+1):
                dp[i][j] = dp[i-1][j]
                if j >= nums[i]:
                    dp[i][j] = max(dp[i][j],dp[i-1][j-nums[i]]+nums[i])

        return dp[len(nums)-1][target] == target
```
> - Time: O(N*Target)
> - Space: O(N*Target)
#### Method 2 一维背包
```python
class Solution:
    def canPartition(self, nums: List[int]) -> bool:
        '''
        背包容量 = sum(nums)/2
        '''
        if sum(nums)%2 == 1:
            return False
        target = int(sum(nums)/2)
        dp = [0] * (target+1)

        for i in range(len(nums)):
            for j in range(target,nums[i]-1,-1):
                dp[j] = max(dp[j],dp[j-nums[i]]+nums[i])
        return dp[target] == target
```
> - Time: O(N*Target)
> - Space: O(Target)
## 今日心得
- 学习一维背包和二维背包。一维背包是二维背包的压缩，把物品的状态压缩，只保留背包容量，更省空间。
