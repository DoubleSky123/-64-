# DAY 6｜242. Valid Anagram 349. Intersection of Two Arrays 202. Happy Number 1. Two Sum
## 学习内容
[哈希表理论基础](https://programmercarl.com/%E5%93%88%E5%B8%8C%E8%A1%A8%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html)
[242讲解](https://programmercarl.com/0242.%E6%9C%89%E6%95%88%E7%9A%84%E5%AD%97%E6%AF%8D%E5%BC%82%E4%BD%8D%E8%AF%8D.html)
[349讲解](https://programmercarl.com/0349.%E4%B8%A4%E4%B8%AA%E6%95%B0%E7%BB%84%E7%9A%84%E4%BA%A4%E9%9B%86.html)
[202讲解](https://programmercarl.com/0202.%E5%BF%AB%E4%B9%90%E6%95%B0.html)
[1讲解](https://programmercarl.com/0001.%E4%B8%A4%E6%95%B0%E4%B9%8B%E5%92%8C.html)
## 242. Valid Anagram
[Leetcode Link](https://leetcode.cn/problems/valid-anagram/description/)-Easy
### Description
>Given two strings s and t, return true if t is an anagram of s, and false otherwise.
>>**Example 1:**
>>**Input:** s = "anagram", t = "nagaram"
>>**Output:** true
### Code
#### Method1 数组哈希表
>题目含义是比较两个字符串是否含有相同字母和字母个数，可以使用数组记录两个字符串字母出现频次，如果频次不同则不同。
```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        hashtable = [0] * 26
        for i in s:
            hashtable[ord(i)-ord('a')] += 1
        for i in t:
            hashtable[ord(i)-ord('a')] -= 1
        for i in hashtable:
            if i != 0:
                return False
        return True
```
> - Time: O(N)
> - Space: O(1)
#### Method2 Counter
>可以直接使用python内置函数Counter()进行比较，代码简洁
```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        cnt_s = Counter(s)
        cnt_t = Counter(t)
        if cnt_s == cnt_t:
            return True
        else:
            return False
```
> - Time: O(N)
> - Space: O(1)
## 349. Intersection of Two Arrays
[Leetcode Link](https://leetcode.cn/problems/intersection-of-two-arrays/description/)-Easy
### Description
>Given two integer arrays nums1 and nums2, return an array of their intersection.
>Each element in the result must be unique and you may return the result in any order.
>>**Example 1:**
>>**Input:** nums1 = `[1,2,2,1]`, nums2 = `[2,2]`
>>**Output:** [2]
### Code
#### Method1 字典哈希表
>del dic[i]可以提高效率，减少无效判断
```python
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        dic = {}
        for i in nums1:
            dic[i] = dic.get(i,0) + 1
        result = set()
        for i in nums2:
            if i in dic:
                result.add(i)
                del dic[i]
        return list(result)
```
> - Time: O(N)
> - Space: O(N)
#### Method2 数组哈希表
```python
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        table1 = [0] * 1001
        table2 = [0] * 1001
        result = []
        for i in nums1:
            table1[i] += 1
        for i in nums2:
            table2[i] += 1

        for i in range(1001):
            if table1[i] * table2[i] > 0:
                result.append(i)
        return result
```
> - Time: O(N)
> - Space: O(N)
## 202. Happy Number
[Leetcode Link](https://leetcode.cn/problems/happy-number/description/)-Easy
### Description
>Write an algorithm to determine if a number n is happy.
>A happy number is a number defined by the following process:
>Starting with any positive integer, replace the number by the sum of the squares of its digits.
>Repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1.
>Those numbers for which this process ends in 1 are happy.
>Return true if n is a happy number, and false if not.
>>**Example 1:**
>>**Input:** n = 19
>>**Output:** true
>>**Explanation:**
>>12 + 92 = 82
>>82 + 22 = 68
>>62 + 82 = 100
>>12 + 02 + 02 = 1
### Code
#### Method1 字典哈希表
>将所有位数平方相加，只有两种情况1是最后为1，2是无限，无限中间一定会有遇到和之前相同的和导致循环。
>时间和空间复杂为logN，因为和给定的数字的位数有关，一般一个数字N的位数即为log10N+1。所以在一开始用record存储这个数字时，空间为logN。在遍历这个数字时要将每一位相加，所以遍历时间为logN。
```python
class Solution:
    def isHappy(self, n: int) -> bool:
        record = set()
        while n not in record:
            record.add(n)
            n_str = str(n)
            n_sum = 0
            for i in n_str:
                n_sum += int(i) **2
            if n_sum == 1:
                return True
            else: n = n_sum
        return False
```
> - Time: O(logN)
> - Space: O(logN)
#### Method2 数组哈希表
```python
class Solution:
    def isHappy(self, n: int) -> bool:
        record = []
        while n not in record:
            record.append(n)
            n_str = str(n)
            n_sum = 0
            for i in n_str:
                n_sum += int(i) ** 2
            if n_sum == 1:
                return True
            else: n = n_sum
        return False
```
> - Time: O(logN)
> - Space: O(logN)
## 1. Two Sum
[Leetcode Link](https://leetcode.cn/problems/two-sum/description/)-Easy
### Description
>Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.
>You may assume that each input would have exactly one solution, and you may not use the same element twice.
>You can return the answer in any order.
>>**Example 1:**
>>**Input:** nums = `[2,7,11,15]`, target = 9
>>**Output:** `[0,1]`
>>**Explanation:** Because nums[0] + nums[1] == 9, we return [0, 1].
### Code
>因为需要存储数组里的值和下标所以用字典最合适。
>如何确定key和value，因为要判断值是否在字典内，所以key是数组中的值，返回的是下标。
>存储值后，判断target-num是否在字典内，如果在则返回两个下标。
```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        dic = {}
        for index,num in enumerate(nums):
            if target- num in dic:
                return[index,dic[target-num]]
            dic[num] = index
            
        return 
```
> - Time: O(N)
> - Space: O(N)
## 今日心得
- 今天开始学习哈希表，学习了三种哈希表的处理方式：`数组、dict、set`，因为dict和set的基本操作其实不是很熟练，就把数组和dict的方式都写了一下。
- 两数之和梦开始的地方，但做的时候出现了一些问题，把dic[num] = index写在了判断前面，这样导致比如target是6，正好数组第一个3，保存3之后，剩下3已经保存在字典中，就会返回错误答案。因此需要先判断。
