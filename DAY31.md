# DAY 31｜56. Merge Intervals 738. Monotone Increasing Digits 968. Binary Tree Cameras
## 学习内容
[贪心算法理论基础](https://programmercarl.com/%E8%B4%AA%E5%BF%83%E7%AE%97%E6%B3%95%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html)
## 56. Merge Intervals
[Leetcode Link](https://leetcode.cn/problems/merge-intervals/description/)-Medium
### Description
>Given an array of intervals where intervals[i] = [starti, endi], merge all overlapping intervals, and return an array of the non-overlapping intervals that cover all the intervals in the input.
>>**Example 1:**
>>**Input:**
>>intervals = [[1,3],[2,6],[8,10],[15,18]]
>>**Output:**
>>[[1,6],[8,10],[15,18]]
>>**Explanation:**
>>Since intervals [1,3] and [2,6] overlap, merge them into [1,6].
### Code
>重叠区间问题，先排序，并且创建一个result数组把第一个区间先加入，用作更新。
>判断当前左边界是否小于上一个区间右边界，如果小于，更新右边界取两个区间的最大值。
>如果大于，直接加入result数组。
```python
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        result = []
        if len(intervals) == 0:
            return result
        intervals.sort(key = lambda x:x[0])

        result.append(intervals[0])

        for i in range(1,len(intervals)):
            if intervals[i][0] <= result[-1][1]:
                result[-1][1] = max(intervals[i][1],result[-1][1])
            else:
                result.append(intervals[i])
        return result
```
> - Time: O(Nlogn)
> - Space: O(N)
## 738. Monotone Increasing Digits
[Leetcode Link](https://leetcode.cn/problems/monotone-increasing-digits/description/)-Medium
### Description
>An integer has monotone increasing digits if and only if each pair of adjacent digits x and y satisfy x <= y.
>Given an integer n, return the largest number that is less than or equal to n with monotone increasing digits.
>>**Example 1:**
>>**Input:**
>>n = 10
>>**Output:**
>>9
### Code
>先把数字转化为字符串进行`倒序`遍历，如果前一位的数字大于后一位数字，说明不是单调递增，把前一位数字-1，然后把后面所有位数字改为9，得出当前最大数。
```python
class Solution:
    def monotoneIncreasingDigits(self, n: int) -> int:
        strNum = list(str(n))

        for i in range(len(strNum)-1,0,-1):
            if strNum[i-1] > strNum[i]:
                strNum[i-1] = str(int(strNum[i - 1]) - 1) 
                for j in range(i,len(strNum)):
                    strNum[j] = '9'
        return int(''.join(strNum))
```
> - Time: O(N)
> - Space: O(N)
## 968. Binary Tree Cameras
[Leetcode Link](https://leetcode.cn/problems/binary-tree-cameras/description/)-Hard
### Description
>You are given the root of a binary tree. We install cameras on the tree nodes where each camera at a node can monitor its parent, itself, and its immediate children.
>Return the minimum number of cameras needed to monitor all nodes of the tree.
>>**Example 1:**
>>**Input:**
>>root = [0,0,null,0,0]
>>**Output:**
>>1
>>**Explanation:**
>>One camera is enough to monitor all nodes if placed as shown.
### Code
```python
## 先跳过
```
> - Time: O(N)
> - Space: O(1)
## 今日心得
- 又训练了重叠区间问题，希望把这个题型可以熟练解决。思路即为先比较当前区间左边界和上一个区间右边界，然后修改当前区间右边界。
- 单调数字问题，注意满足单调且最大即为，除第一位数字外都为9.
