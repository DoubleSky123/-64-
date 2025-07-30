# DAY 44｜1143. Longest Common Subsequence 1035. Uncrossed Lines 53. Maximum Subarray 392. Is Subsequence
## 学习内容
[编辑距离理论基础](https://programmercarl.com/%E4%B8%BA%E4%BA%86%E7%BB%9D%E6%9D%80%E7%BC%96%E8%BE%91%E8%B7%9D%E7%A6%BB%EF%BC%8C%E5%8D%A1%E5%B0%94%E5%81%9A%E4%BA%86%E4%B8%89%E6%AD%A5%E9%93%BA%E5%9E%AB.html)
## 1143. Longest Common Subsequence
[Leetcode Link](https://leetcode.cn/problems/longest-common-subsequence/description/)-Medium
### Description
>Given two strings text1 and text2, return the length of their longest common subsequence. If there is no common subsequence, return 0.
>A subsequence of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.
>For example, "ace" is a subsequence of "abcde". A common subsequence of two strings is a subsequence that is common to both strings.
>>**Example 1:**
>>**Input:**
>>text1 = "abcde", text2 = "ace" 
>>**Output:**
>>3
>>**Explanation:**
>>The longest common subsequence is "ace" and its length is 3.
### Code
> 以i-1,j-1结尾的最长公共子序列长度，判断两个字母相等，即[i-1][j-1]+1，如果不相等，就判断两个字符串前一个字母最长公共子序列取最大。
```python
class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        '''
        [i-1][j-1]的最长公共子序列长度
        '''
        dp = [[0]*(len(text2)+1) for _ in range(len(text1)+1)]

        for i in range(1,len(text1)+1):
            for j in range(1,len(text2)+1):
                if text1[i-1] == text2[j-1]:
                    dp[i][j] = dp[i-1][j-1]+1
                else:
                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])
        return dp[len(text1)][len(text2)]
```
> - Time: O(MN)
> - Space: O(MN)
## 1035. Uncrossed Lines
[Leetcode Link](https://leetcode.cn/problems/uncrossed-lines/description/)-Medium
### Description
>You are given two integer arrays nums1 and nums2. We write the integers of nums1 and nums2 (in the order they are given) on two separate horizontal lines.
>We may draw connecting lines: a straight line connecting two numbers nums1[i] and nums2[j] such that:
>nums1[i] == nums2[j], and the line we draw does not intersect any other connecting (non-horizontal) line.
>Note that a connecting line cannot intersect even at the endpoints (i.e., each number can only belong to one connecting line).
>Return the maximum number of connecting lines we can draw in this way.
>>**Example 1:**
>>**Input:**
>>nums1 = [1,4,2], nums2 = [1,2,4]
>>**Output:**
>>2
>>**Explanation:**
>>We can draw 2 uncrossed lines as in the diagram. We cannot draw 3 uncrossed lines, because the line from nums1[1] = 4 to nums2[2] = 4 will intersect the line from nums1[2]=2 to nums2[1]=2.
### Code
>可以转化为最长公共子序列问题
```python
class Solution:
    def maxUncrossedLines(self, nums1: List[int], nums2: List[int]) -> int:
        dp = [[0]*(len(nums2)+1) for _ in range(len(nums1)+1)]

        for i in range(1,len(nums1)+1):
            for j in range(1,len(nums2)+1):
                if nums1[i-1] == nums2[j-1]:
                    dp[i][j] = dp[i-1][j-1]+1
                else:
                    dp[i][j] = max(dp[i-1][j],dp[i][j-1])
        return dp[-1][-1]
```
> - Time: O(MN)
> - Space: O(MN)
## 53. Maximum Subarray
[Leetcode Link](https://leetcode.cn/problems/maximum-subarray/description/)-Medium
### Description
>Given an integer array nums, find the subarray with the largest sum, and return its sum.
>>**Example 1:**
>>**Input:**
>>nums = [-2,1,-3,4,-1,2,1,-5,4]
>>**Output:**
>>6
>>**Explanation:**
>>The subarray [4,-1,2,1] has the largest sum 6.
### Code
>dp[i] = index i 下的最大子序和，贪心思路：如果前面的和为负，重新取nums[i]，或者dp[i-1]+nums[i]取最大值。
```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        '''
        dp[i] = index i 下的最大子序和
        '''
        dp = [0] * len(nums)
        dp[0] = nums[0]
        res = dp[0]
        for i in range(1,len(nums)):
            dp[i] = max(dp[i-1]+nums[i],nums[i])
            res = max(dp[i],res)
        return res
```
> - Time: O(N)
> - Space: O(N)
## 392. Is Subsequence
[Leetcode Link](https://leetcode.cn/problems/is-subsequence/description/)-Easy
### Description
>Given two strings s and t, return true if s is a subsequence of t, or false otherwise.
>A subsequence of a string is a new string that is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters.
>(i.e., "ace" is a subsequence of "abcde" while "aec" is not).
>>**Example 1:**
>>**Input:**
>>s = "abc", t = "ahbgdc"
>>**Output:**
>>true
>>**Explanation:**
### Code
>最长公共子序列问题，dp数组含义还是求两个数组最长公共子序列长度，如果长度==s返回true。
>因为判断s是否是t的子串，所以判断j-1的位置即可。
```python
class Solution:
    def isSubsequence(self, s: str, t: str) -> bool:
        dp = [[0]*(len(t)+1) for _ in range(len(s)+1)]

        for i in range(1,len(s)+1):
            for j in range(1,len(t)+1):
                if s[i-1] == t[j-1]:
                    dp[i][j] = dp[i-1][j-1]+1
                else:
                    dp[i][j] = dp[i][j-1]
        if dp[-1][-1] == len(s):
            return True
        else:
            return False
```
> - Time: O(MN)
> - Space: O(MN)
## 今日心得
- 最长公共子序列问题，判断两个字母相同和不同的情况，推导递推公式。
