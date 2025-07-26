# DAY 39｜198. House Robber 213. House Robber II 337. House Robber III
## 学习内容
[打家劫舍理论基础](https://programmercarl.com/0198.%E6%89%93%E5%AE%B6%E5%8A%AB%E8%88%8D.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)
## 198. House Robber
[Leetcode Link](https://leetcode.cn/problems/house-robber/description/)-Medium
### Description
>You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed,
>the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and it will automatically contact the police if two adjacent houses were broken into on the same night.
>Given an integer array nums representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police.
>>**Example 1:**
>>**Input:**
>>nums = [1,2,3,1]
>>**Output:**
>>4
>>**Explanation:**
>>Rob house 1 (money = 1) and then rob house 3 (money = 3). Total amount you can rob = 1 + 3 = 4.
### Code
>打家劫舍dp数组表示到第i个房屋偷的最大值，有两种状态偷i和不偷i：
>偷i说明前一个没有偷就是dp[i-2]+nums[i]，不偷i说明偷了前一个就是dp[i-1]，取一个最大值。
```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        '''
        dp[i] = 到第i个house偷的最大money为dp[i]
        偷i：dp[i-2]+nums[i]
        不偷i：dp[i-1]
        '''
        if len(nums) < 2:
            return nums[0]
        dp = [0] *len(nums)
        dp[0] = nums[0]
        dp[1] = max(nums[0],nums[1])

        for i in range(2,len(nums)):
            dp[i] = max(dp[i-2]+nums[i],dp[i-1])

        return dp[-1]
```
> - Time: O(N)
> - Space: O(N)
## 213. House Robber II
[Leetcode Link](https://leetcode.cn/problems/house-robber-ii/description/)-Medium
### Description
>You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed.
>All houses at this place are arranged in a circle. That means the first house is the neighbor of the last one.
>Meanwhile, adjacent houses have a security system connected, and it will automatically contact the police if two adjacent houses were broken into on the same night.
>Given an integer array nums representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police.
>>**Example 1:**
>>**Input:**
>>nums = [2,3,2]
>>**Output:**
>>3
>>**Explanation:**
>>You cannot rob house 1 (money = 2) and then rob house 3 (money = 2), because they are adjacent houses.
### Code
>房子首尾相邻，所以有两种情况：1.抢首不抢尾，2.抢尾不抢首，在这种情况取最大
```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        if len(nums) == 1:
            return nums[0]
        
        # 情况1：抢劫 nums[0] 到 nums[n-2]（不包含最后一个）
        def rob_linear(arr):
            if not arr:
                return 0
            if len(arr) == 1:
                return arr[0]
            
            dp = [0] * len(arr)
            dp[0] = arr[0]
            dp[1] = max(arr[0], arr[1])
            
            for i in range(2, len(arr)):
                dp[i] = max(dp[i-1], dp[i-2] + arr[i])
            
            return dp[-1]
        
        # 比较两种情况：包含第一个房屋 vs 包含最后一个房屋
        return max(rob_linear(nums[:-1]), rob_linear(nums[1:]))
```
> - Time: O(N)
> - Space: O(N)
## 337. House Robber III
[Leetcode Link](https://leetcode.cn/problems/house-robber-iii/description/)-Medium
### Description
>The thief has found himself a new place for his thievery again. There is only one entrance to this area, called root.
>Besides the root, each house has one and only one parent house. After a tour, the smart thief realized that all houses in this place form a binary tree.
>It will automatically contact the police if two directly-linked houses were broken into on the same night.
>Given the root of the binary tree, return the maximum amount of money the thief can rob without alerting the police.
>>**Example 1:**
>>**Input:**
>>root = [3,2,3,null,3,null,1]
>>**Output:**
>>7
>>**Explanation:**
>>Maximum amount of money the thief can rob = 3 + 3 + 1 = 7.
### Code
>树形dp，用后序遍历的方式，先处理叶子节点，分两种情况，1偷和0不偷当前节点，然后再回传给根节点。
>如果不偷当前节点，就取左叶子节点偷和不偷的最大值+右叶子节点偷和不偷的最大值，如果偷当前节点，就取不偷左右叶子节点的值。
>最后取这两种情况的最大值。
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def rob(self, root: Optional[TreeNode]) -> int:

        dp = self.post(root)

        return max(dp)
        
    def post(self,node):
        if not node:
            return (0,0)
        left = self.post(node.left)
        right = self.post(node.right)

        ## 0不偷当前节点
        val0 = max(left[0],left[1])+max(right[0],right[1])
        ## 1偷当前节点
        val1 = node.val + left[0] + right[0]

        return (val0,val1)
```
> - Time: O(N)
> - Space: O(H)
## 今日心得
- 打家劫舍系列，需要注意偷和不偷两种情况的转移。
