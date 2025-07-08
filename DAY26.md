# DAY 26｜455. Assign Cookies 376. Wiggle Subsequence 53. Maximum Subarray
## 学习内容
[贪心算法理论基础](https://programmercarl.com/%E8%B4%AA%E5%BF%83%E7%AE%97%E6%B3%95%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html)
## 455. Assign Cookies
[Leetcode Link](https://leetcode.cn/problems/assign-cookies/description/)-Easy
### Description
>Assume you are an awesome parent and want to give your children some cookies. But, you should give each child at most one cookie.
>Each child i has a greed factor g[i], which is the minimum size of a cookie that the child will be content with; and each cookie j has a size s[j]. If s[j] >= g[i],
>we can assign the cookie j to the child i, and the child i will be content.
>Your goal is to maximize the number of your content children and output the maximum number.
>>**Example 1:**
>>**Input:**
>>g = [1,2,3], s = [1,1]
>>**Output:**
>>1
>>**Explanation:** You have 3 children and 2 cookies. The greed factors of 3 children are 1, 2, 3.
>>And even though you have 2 cookies, since their size is both 1, you could only make the child whose greed factor is 1 content.You need to output 1.
### Code
>贪心策略：最大的饼干喂给胃口最大的孩子或者最小的饼干喂给胃口最小的孩子。
>数组需要排序。然后遍历饼干。
```python
class Solution:
    def findContentChildren(self, g: List[int], s: List[int]) -> int:
        if not s or not g:
            return 0

        g.sort()
        s.sort()

        count = 0
        child = 0
        for cookie in s:
            if child < len(g) and cookie >= g[child]:
                child += 1
                count += 1
        return count
```
> - Time: O(Nlogn)
> - Space: O(1)
## 376. Wiggle Subsequence
[Leetcode Link](https://leetcode.cn/problems/wiggle-subsequence/description/)-Medium
### Description
>A wiggle sequence is a sequence where the differences between successive numbers strictly alternate between positive and negative.
>The first difference (if one exists) may be either positive or negative. A sequence with one element and a sequence with two non-equal elements are trivially wiggle sequences.
>For example, [1, 7, 4, 9, 2, 5] is a wiggle sequence because the differences (6, -3, 5, -7, 3) alternate between positive and negative.
>In contrast, [1, 4, 7, 2, 5] and [1, 7, 4, 5, 5] are not wiggle sequences. The first is not because its first two differences are positive, and the second is not because its last difference is zero.
>A subsequence is obtained by deleting some elements (possibly zero) from the original sequence, leaving the remaining elements in their original order.
>Given an integer array nums, return the length of the longest wiggle subsequence of nums.
>>**Example 1:**
>>**Input:**
>>nums = [1,7,4,9,2,5]
>>**Output:**
>>6
>>**Explanation:** The entire sequence is a wiggle sequence with differences (6, -3, 5, -7, 3).
### Code
>记录前一个坡度和下一个坡度，如果前后坡度不一致，结果+1，并且改变前一个坡度。
>有可能出现单调+平坡，实际坡度只有2.所以前一个坡度不能随着下一个坡度改变而改变。
>初始坡度1。
```python
class Solution:
    def wiggleMaxLength(self, nums: List[int]) -> int:
        if len(nums) == 1:
            return 1

        prediff = curdiff = 0
        ## 初始有一个坡度
        result = 1
        for i in range(len(nums)-1):
            curdiff = nums[i+1]-nums[i]
            if (curdiff >0 and prediff<=0) or (curdiff<0 and prediff >= 0):
                result += 1
                ## 只在坡度改变的情况下记录prediff, 避免单调平坡
                prediff = curdiff
        return result
```
> - Time: O(N)
> - Space: O(1)
## 53. Maximum Subarray
[Leetcode Link](https://leetcode.cn/problems/maximum-subarray/description/)-Medium
### Description
>Given an integer array nums, find the subarray with the largest sum, and return its sum.
>>**Example 1:**
>>**Input:**
>>nums = [-2,1,-3,4,-1,2,1,-5,4]
>>**Output:**
>>6
>>**Explanation:** The subarray [4,-1,2,1] has the largest sum 6.
### Code
>贪心策略：遇到和为负，则清零重新计算和，同时记录最大和。因为一个数加一个负数只会变小。
>从头遍历开始累加。
```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        count = 0
        result = float("-inf")
        for i in nums:
            count += i
            if count > result:
                result = count
            if count < 0:
                count = 0
        return result
```
> - Time: O(N)
> - Space: O(1)
## 今日心得
- 开始贪心算法，没有任何规律，只靠数学思维。
