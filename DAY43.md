# DAY 43｜300. Longest Increasing Subsequence 674. Longest Continuous Increasing Subsequence 718. Maximum Length of Repeated Subarray
## 学习内容
[编辑距离理论基础](https://programmercarl.com/%E4%B8%BA%E4%BA%86%E7%BB%9D%E6%9D%80%E7%BC%96%E8%BE%91%E8%B7%9D%E7%A6%BB%EF%BC%8C%E5%8D%A1%E5%B0%94%E5%81%9A%E4%BA%86%E4%B8%89%E6%AD%A5%E9%93%BA%E5%9E%AB.html)
## 300. Longest Increasing Subsequence
[Leetcode Link](https://leetcode.cn/problems/longest-increasing-subsequence/description/)-Medium
### Description
>Given an integer array nums, return the length of the longest strictly increasing subsequence.
>>**Example 1:**
>>**Input:**
>>nums = [10,9,2,5,3,7,101,18]
>>**Output:**
>>4
>>**Explanation:**
>>The longest increasing subsequence is [2,3,7,101], therefore the length is 4.
### Code
>dp[i] = 到i这个位置前面最长的递增子序列长度，j为i之前的数，如果i>j，说明dp[j]+1，j是遍历i之前的所有数，所以取一个最大的dp[i]。
>i所在位置的dp[i]不一定是最大值，所以需要一个变量记录最大值。
```python
class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        if len(nums)<=1:
            return 1
        dp = [1] * len(nums)
        res = 0
        for i in range(1,len(nums)):
            for j in range(i):
                if nums[i] > nums[j]:
                    dp[i] = max(dp[j]+1,dp[i])
            res = max(dp[i],res)
        return res
```
> - Time: O(N^2)
> - Space: O(N)
## 674. Longest Continuous Increasing Subsequence
[Leetcode Link](https://leetcode.cn/problems/longest-continuous-increasing-subsequence/description/)-Easy
### Description
>Given an unsorted array of integers nums, return the length of the longest continuous increasing subsequence (i.e. subarray). The subsequence must be strictly increasing.
>A continuous increasing subsequence is defined by two indices l and r (l < r) such that it is [nums[l], nums[l + 1], ..., nums[r - 1], nums[r]] and for each l <= i < r, nums[i] < nums[i + 1].
>>**Example 1:**
>>**Input:**
>>nums = [1,3,5,4,7]
>>**Output:**
>>3
>>**Explanation:**
>>The longest continuous increasing subsequence is [1,3,5] with length 3. Even though [1,3,5,7] is an increasing subsequence, it is not continuous as elements 5 and 7 are separated by element 4.
### Code
>连续递增子序列，比较i和i-1，如果i>i-1，dp[i]=dp[i-1]+1，同时用一个变量记录最大值
```python
class Solution:
    def findLengthOfLCIS(self, nums: List[int]) -> int:
        if len(nums)<=1:
            return 1
        dp = [1] * len(nums)
        res = 0
        for i in range(1,len(nums)):
            if nums[i]>nums[i-1]:
                dp[i] = dp[i-1]+1
            res = max(dp[i],res)
        return res
```
> - Time: O(N)
> - Space: O(N)
## 718. Maximum Length of Repeated Subarray
[Leetcode Link](https://leetcode.cn/problems/maximum-length-of-repeated-subarray/description/)-Medium
### Description
>Given two integer arrays nums1 and nums2, return the maximum length of a subarray that appears in both arrays.
>>**Example 1:**
>>**Input:**
>>nums1 = [1,2,3,2,1], nums2 = [3,2,1,4,7]
>>**Output:**
>>3
>>**Explanation:**
>>The repeated subarray with maximum length is [3,2,1].
### Code
>二维数组， dp[i][j] 以i-1,j-1为结尾的最长连续重复子序列，当i-1的值==j-1，说明dp[i][j]的最长连续重复子序列在前一位的基础上+1.
>以i-1,j-1为结尾，可以在初始化时，都初始化为0，如果以i，j结尾，在初始化dp[i][0],[0][j]的时候需要判断第一个字母和另外一个数组比较。而i-1，j-1本身无意义，所以初始化为0.
```python
class Solution:
    def findLength(self, nums1: List[int], nums2: List[int]) -> int:
        '''
        dp[i-1][j-1] 以i-1,j-1为结尾的最长连续重复子序列 
        '''
        dp = [[0] * (len(nums2) + 1) for _ in range(len(nums1) + 1)]
        res = 0
        for i in range(1,len(nums1)+1):
            for j in range(1,len(nums2)+1):
                if nums1[i-1] == nums2[j-1]:
                    dp[i][j] = dp[i-1][j-1]+1
                res = max(res,dp[i][j])
        return res
```
> - Time: O(MN)
> - Space: O(MN)
## 今日心得
- 子序列问题，以及编辑距离入门，当给出判断两个数组的时候，需要二维数组，并且注意定义是i-1,j-1为结尾，方便初始化，所以遍历时长度+1.
