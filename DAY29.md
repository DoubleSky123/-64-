# DAY 29｜452. Minimum Number of Arrows to Burst Balloons 435. Non-overlapping Intervals 763. Partition Labels
## 学习内容
[贪心算法理论基础](https://programmercarl.com/%E8%B4%AA%E5%BF%83%E7%AE%97%E6%B3%95%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html)
## 452. Minimum Number of Arrows to Burst Balloons
[Leetcode Link](https://leetcode.cn/problems/minimum-number-of-arrows-to-burst-balloons/description/)-Medium
### Description
>There are some spherical balloons taped onto a flat wall that represents the XY-plane.
>The balloons are represented as a 2D integer array points where points[i] = [xstart, xend] denotes a balloon whose horizontal diameter stretches between xstart and xend.
>You do not know the exact y-coordinates of the balloons.
>Arrows can be shot up directly vertically (in the positive y-direction) from different points along the x-axis. A balloon with xstart and xend is burst by an arrow shot at x if xstart <= x <= xend.
>There is no limit to the number of arrows that can be shot. A shot arrow keeps traveling up infinitely, bursting any balloons in its path.
>Given the array points, return the minimum number of arrows that must be shot to burst all balloons.
>>**Example 1:**
>>**Input:**
>>points = [[10,16],[2,8],[1,6],[7,12]]
>>**Output:**
>>2
>>**Explanation:** The balloons can be burst by 2 arrows:
>>Shoot an arrow at x = 6, bursting the balloons [2,8] and [1,6].
>>Shoot an arrow at x = 11, bursting the balloons [10,16] and [7,12].
### Code
>重叠区间问题，先排序，遍历如果当前区间左边界大于上一个区间右边界说明不重叠。
>然后更新区间的右边界，取当前区间和上一个区间右边界最小值。
```python
class Solution:
    def findMinArrowShots(self, points: List[List[int]]) -> int:
        if len(points) == 0:
            return 0
        points.sort(key = lambda x: x[0])
        result = 1

        for i in range(1,len(points)):
            if points[i][0] > points[i-1][1]:
                result += 1
            else:
                points[i][1] = min(points[i][1],points[i-1][1])
        
        return result
```
> - Time: O(Nlogn)
> - Space: O(1)
## 435. Non-overlapping Intervals
[Leetcode Link](https://leetcode.cn/problems/non-overlapping-intervals/description/)-Medium
### Description
>Given an array of intervals intervals where intervals[i] = [starti, endi], return the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping.
>Note that intervals which only touch at a point are non-overlapping. For example, [1, 2] and [2, 3] are non-overlapping.
>>**Example 1:**
>>**Input:**
>>intervals = [[1,2],[2,3],[3,4],[1,3]]
>>**Output:**
>>1
>>**Explanation:** [1,3] can be removed and the rest of the intervals are non-overlapping.
### Code
>同样是重叠区间问题，注意这里的条件是可以刚好重叠。
```python
class Solution:
    def eraseOverlapIntervals(self, intervals: List[List[int]]) -> int:
        if len(intervals) == 0:
            return 0
        intervals.sort(key = lambda x:x[0])

        result= 0
        for i in range(1,len(intervals)):
            if intervals[i][0]>=intervals[i-1][1]:
                continue
            else:
                result += 1
                intervals[i][1] = min(intervals[i-1][1],intervals[i][1])
        return result
```
> - Time: O(Nlogn)
> - Space: O(1)
## 763. Partition Labels
[Leetcode Link](https://leetcode.cn/problems/partition-labels/description/)-Medium
### Description
>You are given a string s. We want to partition the string into as many parts as possible so that each letter appears in at most one part.
>For example, the string "ababcc" can be partitioned into ["abab", "cc"], but partitions such as ["aba", "bcc"] or ["ab", "ab", "cc"] are invalid.
>Note that the partition is done so that after concatenating all the parts in order, the resultant string should be s.
>Return a list of integers representing the size of these parts.
>>**Example 1:**
>>**Input:**
>>s = "ababcbacadefegdehijhklij"
>>**Output:**
>>[9,7,8]
>>**Explanation:** The partition is "ababcbaca", "defegde", "hijhklij".This is a partition so that each letter appears in at most one part.A partition like "ababcbacadefegde", "hijhklij" is incorrect, because it splits s into less parts.
### Code
>有点像滑动窗口，
>先遍历一边字符串，用一个数组记录每一个字母的最远下标。
>用两个指针，当右指针指向最远下标，并且遍历到右指针时，获取结果，并且左指针移动到下一位。
```python
class Solution:
    def partitionLabels(self, s: str) -> List[int]:
        char_pos = [0] * 26
        for i in range(len(s)):
            char_pos[ord(s[i])-ord('a')] =i

        result = []
        left,right = 0,0
        for i in range(len(s)):
            right = max(char_pos[ord(s[i])-ord('a')],right)
            
            if i == right:
                result.append((right-left+1))
                left = i+1
        return result
```
> - Time: O(N)
> - Space: O(1)
## 今日心得
- 两道重叠区间问题，关键是更新右边界取当前和上一个区间的最小值。第三题类似滑动窗口，关键是先找到每一个字母的最远下标。
