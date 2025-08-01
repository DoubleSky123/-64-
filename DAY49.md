# DAY 49｜42. Trapping Rain Water 84. Largest Rectangle in Histogram
## 学习内容
[单调栈理论基础](https://programmercarl.com/0739.%E6%AF%8F%E6%97%A5%E6%B8%A9%E5%BA%A6.html)
## 42. Trapping Rain Water
[Leetcode Link](https://leetcode.cn/problems/trapping-rain-water/description/)-Hard
### Description
>Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it can trap after raining.
>>**Example 1:**
>>**Input:**
>>height = [0,1,0,2,1,0,1,3,2,1,2,1]
>>**Output:**
>>6
>>**Explanation:**
>>The above elevation map (black section) is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped.
### Code
>单调栈求面积问题，框架还是单调递增栈存index，当数值相等时弹出避免多一步运算。
>碰到大于栈口元素时，弹出栈口元素，这个元素是作为接雨水的底，两边是高，将当前元素和下一个栈口元素取最小-弹出元素就是高。
```python
class Solution:
    def trap(self, height: List[int]) -> int:
        stack = [0]
        res = 0
        for i in range(1,len(height)):
            if height[i] < height[stack[-1]]:
                stack.append(i)
            elif height[i] == height[stack[-1]]:
                stack.pop()
                stack.append(i)
            else:
                while stack and height[i]>height[stack[-1]]:
                    mid = stack.pop()
                    if stack:
                        h = min(height[i],height[stack[-1]])-height[mid]
                        w = i-stack[-1]-1
                        res += h*w
                stack.append(i)
        return res
```
> - Time: O(N)
> - Space: O(N)
## 84. Largest Rectangle in Histogram
[Leetcode Link](https://leetcode.cn/problems/largest-rectangle-in-histogram/description/)-Hard
### Description
>Given an array of integers heights representing the histogram's bar height where the width of each bar is 1, return the area of the largest rectangle in the histogram.
>>**Example 1:**
>>**Input:**
>>heights = [2,1,5,6,2,3]
>>**Output:**
>>10
>>**Explanation:**
>>The above is a histogram where width of each bar is 1. The largest rectangle is shown in the red area, which has an area = 10 units.
### Code
>和上题一样还是求面积，用单调递减栈。
```python
class Solution:
    def largestRectangleArea(self, heights: List[int]) -> int:
        heights.insert(0,0)
        heights.append(0)
        stack = [0]
        res = 0
        for i in range(1,len(heights)):
            if heights[i] > heights[stack[-1]]:
                stack.append(i)
            elif heights[i] == heights[stack[-1]]:
                stack.pop()
                stack.append(i)
            else:
                while stack and heights[i] < heights[stack[-1]]:
                    mid = stack.pop()
                    if stack:
                        left = stack[-1]
                        right = i 
                        w = right-left -1
                        h = heights[mid]
                        res = max(res,w*h)
                        print(res,left,right,mid)
                stack.append(i)
        return res
```
> - Time: O(N)
> - Space: O(N)
## 今日心得
- 单调栈求面积，难点在于面积的计算，第一个弹出的值是中间值。以及碰到相同元素弹出。
