# DAY 25｜491. Non-decreasing Subsequences 46. Permutations 47. Permutations II
## 学习内容
[回溯算法理论基础](https://programmercarl.com/%E5%9B%9E%E6%BA%AF%E7%AE%97%E6%B3%95%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html)
## 491. Non-decreasing Subsequences
[Leetcode Link](https://leetcode.cn/problems/non-decreasing-subsequences/description/)-Medium
### Description
>Given an integer array nums, return all the different possible non-decreasing subsequences of the given array with at least two elements. You may return the answer in any order.
>>**Example 1:**
>>**Input:**
>>nums = [4,6,7,7]
>>**Output:**
>>[[4,6],[4,6,7],[4,6,7,7],[4,7],[4,7,7],[6,7],[6,7,7],[7,7]]
### Code
>递增子序列需要注意去重不能先把数组排序，排序会把数组打乱导致结果错误。
>需要用set去重，把出现过的数字加入set，出现了就continue。因为子序列是递增，所以数字比较和path[-1]最后一位比较。
```python
class Solution:
    def findSubsequences(self, nums: List[int]) -> List[List[int]]:
        result = []
        self.backtracking(nums,[],result,0)
        return result
        
    def backtracking(self,nums,path,result,startIndex):
        if len(path) >1:
            result.append(path[:])
        uset = set()
        for i in range(startIndex,len(nums)):
            if (path and nums[i] < path[-1]) or nums[i] in uset:
                continue
            uset.add(nums[i])
            path.append(nums[i])
            self.backtracking(nums,path,result,i+1)
            path.pop()
```
> - Time: O(N^2)
> - Space: O(N)
## 46. Permutations
[Leetcode Link](https://leetcode.cn/problems/permutations/description/)-Medium
### Description
>Given an array nums of distinct integers, return all the possible permutations. You can return the answer in any order.
>>**Example 1:**
>>**Input:**
>>nums = [1,2,3]
>>**Output:**
>>[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
### Code
>全排列不用startIndex，每次for循环遍历都是从0开始。
>但是需要用used数组去除已经选取过的数字，剩下的数字再继续选取。
```python
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        result = []
        self.backtracking(nums,[False]*len(nums),[],result)
        return result

    def backtracking(self,nums,used,path,result):
        if len(path) == len(nums):
            result.append(path[:])
            return 
        for i in range(len(nums)):
            if used[i] == True:
                continue
            used[i] = True
            path.append(nums[i])
            self.backtracking(nums,used,path,result)
            path.pop()
            used[i] = False
```
> - Time: O(N!)
> - Space: O(N)
## 47. Permutations II
[Leetcode Link](https://leetcode.cn/problems/permutations-ii/description/)-Medium
### Description
>Given a collection of numbers, nums, that might contain duplicates, return all possible unique permutations in any order.
>>**Example 1:**
>>**Input:**
>>nums = [1,1,2]
>>**Output:**
>>[[1,1,2],[1,2,1],[2,1,1]]
### Code
>全排列去重也是树层上的去重，所以用used数组，used[i-1] == False。
>如果前一位没有被选取过但数值相同说明是树层上相同。
```python
class Solution:
    def permuteUnique(self, nums: List[int]) -> List[List[int]]:
        result = []
        nums.sort()
        self.backtracking(nums,[False]*len(nums),[],result)
        return result

    def backtracking(self,nums,used,path,result):
        if len(path) == len(nums):
            result.append(path[:])
            return
        for i in range(len(nums)):
            if used[i] == True or (i > 0 and used[i-1] == False and nums[i] == nums[i - 1]):
                continue

            used[i] = True
            path.append(nums[i])
            self.backtracking(nums,used,path,result)
            path.pop()
            used[i] = False
```
> - Time: O(N!)
> - Space: O(N)
## 今日心得
- 回溯最后一天，学习了排列问题以及用set和used数组去重。
- 当这些问题不能使用startIndex遍历或者数组不能排序时，则需要用set或者used数组去重。
- 最后三题hard题后面补上。
