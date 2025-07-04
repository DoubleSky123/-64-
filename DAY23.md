# DAY 23｜39. Combination Sum 40. Combination Sum II 131. Palindrome Partitioning
## 学习内容
[回溯算法理论基础](https://programmercarl.com/%E5%9B%9E%E6%BA%AF%E7%AE%97%E6%B3%95%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html)
## 39. Combination Sum
[Leetcode Link](https://leetcode.cn/problems/combination-sum/description/)-Medium
### Description
>Given an array of distinct integers candidates and a target integer target, return a list of all unique combinations of candidates where the chosen numbers sum to target.
>You may return the combinations in any order.
>The same number may be chosen from candidates an unlimited number of times. Two combinations are unique if the frequency of at least one of the chosen numbers is different.
>The test cases are generated such that the number of unique combinations that sum up to target is less than 150 combinations for the given input.
>>**Example 1:**
>>**Input:**
>>candidates = [2,3,6,7], target = 7
>>**Output:**
>>[[2,2,3],[7]]
>>**Explanation:** 2 and 3 are candidates, and 2 + 2 + 3 = 7. Note that 2 can be used multiple times.7 is a candidate, and 7 = 7.These are the only two combinations.
### Code
>区别是数组中每个数组都可以重复使用，所以每次递归startIndex都从0开始不需要+1.
```python
class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        result = []
        self.backtracking(candidates,target,[],result,0)
        return result

    def backtracking(self,candidates,target,path,result,startIndex):
        if target< 0:
            return
        if target == 0:
            result.append(path[:])
            return
        for i in range(startIndex,len(candidates)):
            path.append(candidates[i])
            self.backtracking(candidates,target-candidates[i],path,result,i)
            path.pop()
```
> - Time: O(2^N)
> - Space: O(N)
## 40. Combination Sum II
[Leetcode Link](https://leetcode.cn/problems/combination-sum-ii/description/)-Medium
### Description
>Given a collection of candidate numbers (candidates) and a target number (target), find all unique combinations in candidates where the candidate numbers sum to target.
>Each number in candidates may only be used once in the combination.
>Note: The solution set must not contain duplicate combinations.
>>**Example 1:**
>>**Input:**
>>candidates = [10,1,2,7,6,1,5], target = 8
>>**Output:**
>>[[1,1,6],[1,2,5],[1,7],[2,6]]
### Code
>区别是这里数组中的数字重复了，递归时会出现重复集合。
>去重先要将数组排序，然后在同一层中（for循环）如果后面的数字和前面相等直接continue。
```python
class Solution:
    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
        result = []
        candidates.sort()
        self.backtracking(candidates,target,[],result,0)
        return result

    def backtracking(self,candidates,target,path,result,startIndex):
        if target < 0:
            return 
        if target == 0:
            result.append(path[:])
            return 
        for i in range(startIndex,len(candidates)):
            if i > startIndex and candidates[i] == candidates[i-1]:
                continue
            if candidates[i] > target:
                break
            path.append(candidates[i])
            self.backtracking(candidates,target-candidates[i],path,result,i+1)
            path.pop()
```
> - Time: O(2^N)
> - Space: O(N)
## 131. Palindrome Partitioning
[Leetcode Link](https://leetcode.cn/problems/palindrome-partitioning/description/)-Medium
### Description
>Given a string s, partition s such that every substring of the partition is a palindrome. Return all possible palindrome partitioning of s.
>>**Example 1:**
>>**Input:**
>>s = "aab"
>>**Output:**
>>[["a","a","b"],["aa","b"]]
### Code
>回溯中切割字符串，用startIndex作为切割线去切割。再判断是否是回文串，双指针或者切片即可。
>终止条件为startIndex遍历到数组末尾，因为收集结果都是在字符串遍历完之后，但是字符串切割的范围是[startIndex,i]，每切割一次path中收集一次。
```python
class Solution:
    def partition(self, s: str) -> List[List[str]]:
        result = []
        self.backtracking(s,[],result,0)
        return result

    def backtracking(self,s,path,result,startIndex):
        if startIndex == len(s):
            result.append(path[:])
            return 
        for i in range(startIndex,len(s)):
            if s[startIndex:i+1] == s[startIndex:i+1][::-1]:
                path.append(s[startIndex:i+1])

                self.backtracking(s,path,result,i+1)
                path.pop()
```
> - Time: O(2^N)
> - Space: O(N^2)
## 今日心得
- 今天回溯学习了数组中如果出现重复数字先排序再从同一层中去重。
- startIndex作为切割线时，切割范围是[startIndex,i]。
