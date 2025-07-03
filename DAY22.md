# DAY 22｜77. Combinations 216. Combination Sum III 17. Letter Combinations of a Phone Number
## 学习内容
[回溯算法理论基础](https://programmercarl.com/%E5%9B%9E%E6%BA%AF%E7%AE%97%E6%B3%95%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html)
## 77. Combinations
[Leetcode Link](https://leetcode.cn/problems/combinations/description/)-Medium
### Description
>Given two integers n and k, return all possible combinations of k numbers chosen from the range [1, n].You may return the answer in any order.
>>**Example 1:**
>>**Input:**
>>n = 4, k = 2
>>**Output:**
>>[[1,2],[1,3],[1,4],[2,3],[2,4],[3,4]]
>>**Explanation:** There are 4 choose 2 = 6 total combinations.Note that combinations are unordered, i.e., [1,2] and [2,1] are considered to be the same combination.
### Code
```python
class Solution:
    def combine(self, n: int, k: int) -> List[List[int]]:
        result = []
        self.backtracking(n,k,1,[],result)
        return result

    def backtracking(self,n,k,startIndex,path,result):
        if len(path)==k:
            result.append(path[:])
            return

        for i in range(startIndex,n-(k-len(path))+2):
            ##表示最后一个有效的起始位置为n-k+1
            ##for循环里面的i遍历的都是作为起始位置的值，横向遍历
            path.append(i)
            self.backtracking(n,k,i+1,path,result)
            ##递归表示向下遍历
            path.pop()
```
> - Time: O(C(N,K))
> - Space: O(K)
## 216. Combination Sum III
[Leetcode Link](https://leetcode.cn/problems/combination-sum-iii/description/)-Medium
### Description
>Find all valid combinations of k numbers that sum up to n such that the following conditions are true:Only numbers 1 through 9 are used.
>Each number is used at most once.Return a list of all possible valid combinations. The list must not contain the same combination twice, and the combinations may be returned in any order.
>>**Example 1:**
>>**Input:**
>>k = 3, n = 7
>>**Output:**
>>[[1,2,4]]
>>**Explanation:** 1 + 2 + 4 = 7 There are no other valid combinations.
### Code
```python
class Solution:
    def combinationSum3(self, k: int, n: int) -> List[List[int]]:
        result = []
        self.backtracking(k,n,1,[],result)
        return result

    def backtracking(self,k,n,startIndex,path,result):
        ##剪枝：如果和已经大于n，直接返回
        if n < 0:
            return 
        if len(path) == k:
            if n == 0:
                result.append(path[:])
                return
        ##剪枝：限定最后的起始位置
        for i in range(startIndex,9-(k-len(path))+2):
            if i > n:
                break
            path.append(i)
            self.backtracking(k,n-i,i+1,path,result)
            path.pop()
```
> - Time: O(N * 2^N)
> - Space: O(N)
## 17. Letter Combinations of a Phone Number
[Leetcode Link](https://leetcode.cn/problems/letter-combinations-of-a-phone-number/description/)-Medium
### Description
>Given a string containing digits from 2-9 inclusive, return all possible letter combinations that the number could represent. Return the answer in any order.
>A mapping of digits to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.
>>**Example 1:**
>>**Input:**
>>digits = "23"
>>**Output:**
>>["ad","ae","af","bd","be","bf","cd","ce","cf"]
### Code
```python
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        if not digits:
            return []
        # 预处理
        lettermap = ["", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"]
        digits_int = [int(d) for d in digits]  # 预先转换
        result = []
        path = []
        
        def backtrack(index):
            if index == len(digits_int):
                result.append(''.join(path))
                return
            
            for letter in lettermap[digits_int[index]]:
                path.append(letter)
                backtrack(index + 1)
                path.pop()
        
        backtrack(0)
        return result
```
> - Time: O(3^N)
> - Space: O(N)
## 今日心得
- 回溯中的组合问题用回溯三部曲和模板（startIndex+for循环）
- 组合问题可以剪枝，剪在单层for循环中，表示横向遍历可以减少。n-(k-len(path))+1，表示最后一个有效起点位置
