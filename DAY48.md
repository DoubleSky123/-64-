# DAY 48｜739. Daily Temperatures 496. Next Greater Element I 503. Next Greater Element II
## 学习内容
[单调栈理论基础](https://programmercarl.com/0739.%E6%AF%8F%E6%97%A5%E6%B8%A9%E5%BA%A6.html)
## 739. Daily Temperatures
[Leetcode Link](https://leetcode.cn/problems/daily-temperatures/description/)-Medium
### Description
>Given an array of integers temperatures represents the daily temperatures, return an array answer such that answer[i] is the number of days you have to wait after the ith day to get a warmer temperature.
>If there is no future day for which this is possible, keep answer[i] == 0 instead.
>>**Example 1:**
>>**Input:**
>>temperatures = [73,74,75,71,69,72,76,73]
>>**Output:**
>>[1,1,4,2,1,1,0,0]
### Code
>单调递增栈，把index入栈，如果小于栈口元素入栈，如果大于栈口元素出栈，并加入结果。
```python
class Solution:
    def dailyTemperatures(self, temperatures: List[int]) -> List[int]:
        res = [0]*len(temperatures)
        stack = []
        for i in range(len(temperatures)):
            while stack and temperatures[i] > temperatures[stack[-1]]:
                res[stack[-1]] = i-stack[-1]
                stack.pop()
            stack.append(i)
        return res
```
> - Time: O(N)
> - Space: O(N)
## 496. Next Greater Element I
[Leetcode Link](https://leetcode.cn/problems/next-greater-element-i/description/)-Easy
### Description
>The next greater element of some element x in an array is the first greater element that is to the right of x in the same array.
>You are given two distinct 0-indexed integer arrays nums1 and nums2, where nums1 is a subset of nums2.
>For each 0 <= i < nums1.length, find the index j such that nums1[i] == nums2[j] and determine the next greater element of nums2[j] in nums2. If there is no next greater element, then the answer for this query is -1.
>Return an array ans of length nums1.length such that ans[i] is the next greater element as described above.
>>**Example 1:**
>>**Input:**
>>nums1 = [4,1,2], nums2 = [1,3,4,2]
>>**Output:**
>>[-1,3,-1]
>>**Explanation:**
>>The next greater element for each value of nums1 is as follows:- 4 is underlined in nums2 = [1,3,4,2]. There is no next greater element, so the answer is -1.
>>- 1 is underlined in nums2 = [1,3,4,2]. The next greater element is 3.- 2 is underlined in nums2 = [1,3,4,2]. There is no next greater element, so the answer is -1.
### Code
>需要匹配，所以先把nums1存到字典中，然后用单调递增栈，和上题方法相同。
```python
class Solution:
    def nextGreaterElement(self, nums1: List[int], nums2: List[int]) -> List[int]:
        res = [-1] * len(nums1)
        stack = []
        nums1_map = {num: i for i, num in enumerate(nums1)}

        for i in range(len(nums2)):
            while stack and nums2[i] > nums1[stack[-1]]:
                res[stack[-1]] = nums2[i]
                stack.pop()
            
            if nums2[i] in nums1_map:
                stack.append(nums1_map[nums2[i]])
        return res
```
> - Time: O(N)
> - Space: O(N)
## 503. Next Greater Element II
[Leetcode Link](https://leetcode.cn/problems/next-greater-element-ii/description/)-Medium
### Description
>Given a circular integer array nums (i.e., the next element of nums[nums.length - 1] is nums[0]), return the next greater number for every element in nums.
>The next greater number of a number x is the first greater number to its traversing-order next in the array, which means you could search circularly to find its next greater number. If it doesn't exist, return -1 for this number.
>>**Example 1:**
>>**Input:**
>>nums = [1,2,1]
>>**Output:**
>>[2,-1,2]
>>**Explanation:**
>>The first 1's next greater number is 2; The number 2 can't find next greater number. The second 1's next greater number needs to search circularly, which is also 2.
### Code
>环形数组，只需要将数组转化为线性，用index%len(nums)，就可以循环两次数组，从而找到。
```python
class Solution:
    def nextGreaterElements(self, nums: List[int]) -> List[int]:
        res = [-1]*len(nums)
        stack = []
        n= len(nums)
        for i in range(n*2):
            while stack and nums[i%n] > nums[stack[-1]]:
                res[stack[-1]] = nums[i%n]
                stack.pop()
            if i < n:
                stack.append(i%n)
        return res
```
> - Time: O(N)
> - Space: O(N)
## 今日心得
- 单调栈练习，题目一般是找下一个更大的数，就用单调递增栈，并且栈内存储的是index。
