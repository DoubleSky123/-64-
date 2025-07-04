# DAY 24｜93. Restore IP Addresses 78. Subsets 90. Subsets II
## 学习内容
[回溯算法理论基础](https://programmercarl.com/%E5%9B%9E%E6%BA%AF%E7%AE%97%E6%B3%95%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html)
## 93. Restore IP Addresses
[Leetcode Link](https://leetcode.cn/problems/restore-ip-addresses/description/)-Medium
### Description
>A valid IP address consists of exactly four integers separated by single dots. Each integer is between 0 and 255 (inclusive) and cannot have leading zeros.
>For example, "0.1.2.201" and "192.168.1.1" are valid IP addresses, but "0.011.255.245", "192.168.1.312" and "192.168@1.1" are invalid IP addresses.
>Given a string s containing only digits, return all possible valid IP addresses that can be formed by inserting dots into s.
> You are not allowed to reorder or remove any digits in s. You may return the valid IP addresses in any order.
>>**Example 1:**
>>**Input:**
>>s = "25525511135"
>>**Output:**
>>["255.255.11.135","255.255.111.35"]
### Code
>ip地址的终止条件为，当有三个"."的时候终止，所以需要一个变量记录"."的数量。
>同时当"."数量为3时，最后一段ip还没有判断需要再判断一下才能收获结果。
>额外写一个函数判断数字是否合法，同时切割范围也是[startIndex:i]，因为是同一层判断。
```python
class Solution:
    def restoreIpAddresses(self, s: str) -> List[str]:
        result = []
        self.backtracking(s,0,0,result,'')
        return result

    def backtracking(self,s,startIndex,pointSum,result,cur_str):
        
        def isValid(s,start,end):
            if start > end:
                return False
            if s[start] == '0' and start != end:  # 0开头的数字不合法
                return False
            num = 0
            for i in range(start, end + 1):
                if not s[i].isdigit():  # 遇到非数字字符不合法
                    return False
                num = num * 10 + int(s[i])
                if num > 255:  # 如果大于255了不合法
                    return False
            return True

        if pointSum == 3:
            if isValid(s,startIndex,len(s)-1):
                cur_str += s[startIndex:]
                result.append(cur_str)
                return

        for i in range(startIndex,len(s)):
            if isValid(s,startIndex,i):
                sub = s[startIndex:i+1]
                self.backtracking(s,i+1,pointSum+1,result,cur_str+sub+'.')
            else:
                break
```
> - Time: O(3^N)
> - Space: O(N)
## 78. Subsets
[Leetcode Link](https://leetcode.cn/problems/subsets/description/)-Medium
### Description
>Given an integer array nums of unique elements, return all possible subsets (the power set).
>The solution set must not contain duplicate subsets. Return the solution in any order.
>>**Example 1:**
>>**Input:**
>>nums = [1,2,3]
>>**Output:**
>>[[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
### Code
>子集问题，最大区别是不是在终止条件收获结果，而是每一次递归收获结果，所以没有终止，收获结果在for循环前面。
```python
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        result = []
        self.backtracking(nums,[],result,0)
        return result

    def backtracking(self,nums,subset,result,Index):
        ## 收获结果
        result.append(subset[:])
        
        for i in range(Index,len(nums)):
            subset.append(nums[i])
            self.backtracking(nums,subset,result,i+1)
            subset.pop()
```
> - Time: O(2^N)
> - Space: O(N)
## 90. Subsets II
[Leetcode Link](https://leetcode.cn/problems/subsets-ii/description/)-Medium
### Description
>Given an integer array nums that may contain duplicates, return all possible subsets (the power set).
>The solution set must not contain duplicate subsets. Return the solution in any order.
>>**Example 1:**
>>**Input:**
>>nums = [1,2,2]
>>**Output:**
>>[[],[1],[1,2],[1,2,2],[2],[2,2]]
### Code
>子集去重，也是在同一层去重即i > Index。
```python
class Solution:
    def subsetsWithDup(self, nums: List[int]) -> List[List[int]]:
        result = []
        nums.sort()
        self.backtracking(nums,[],result,0)
        return result

    def backtracking(self,nums,path,result,Index):
        result.append(path[:])

        for i in range(Index,len(nums)):
            if i > Index and nums[i]== nums[i-1]:
                continue
            path.append(nums[i])
            self.backtracking(nums,path,result,i+1)
            path.pop()
```
> - Time: O(N*2^N)
> - Space: O(N)
## 今日心得
- ip切割问题，需要注意的是终止条件，以及终止后收获结果前的最后一次判断，以及如何判断数字合法。
- 子集问题，没有终止，在每次递归前收获结果。
