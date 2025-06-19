# DAY 9｜151. Reverse Words in a String 55. 右旋字符串 28. Find the Index of the First Occurrence in a String 459. Repeated Substring Pattern
## 学习内容
[151讲解](https://programmercarl.com/0151.%E7%BF%BB%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2%E9%87%8C%E7%9A%84%E5%8D%95%E8%AF%8D.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)
[右旋字符串讲解](https://programmercarl.com/kamacoder/0055.%E5%8F%B3%E6%97%8B%E5%AD%97%E7%AC%A6%E4%B8%B2.html)
[28讲解](https://programmercarl.com/0028.%E5%AE%9E%E7%8E%B0strStr.html)
[459讲解](https://programmercarl.com/0459.%E9%87%8D%E5%A4%8D%E7%9A%84%E5%AD%90%E5%AD%97%E7%AC%A6%E4%B8%B2.html)
## 151. Reverse Words in a String
[Leetcode Link](https://leetcode.cn/problems/reverse-words-in-a-string/description/)-Medium
### Description
>Given an input string s, reverse the order of the words.
>A word is defined as a sequence of non-space characters. The words in s will be separated by at least one space.
>Return a string of the words in reverse order concatenated by a single space.
>Note that s may contain leading or trailing spaces or multiple spaces between two words.
>The returned string should only have a single space separating the words. Do not include any extra spaces.
>>**Example 1:**
>>**Input:**
>>s = "the sky is blue"
>>**Output:**
>>"blue is sky the"
### Code
>将string转化为list秒了，自带去除空格。
```python
class Solution:
    def reverseWords(self, s: str) -> str:
        wordlist = list(s.split())
        left,right = 0, len(wordlist)-1
        ## wordlist = words[::-1] 直接反转列表
        while left < right:
            wordlist[left], wordlist[right] = wordlist[right], wordlist[left]
            left += 1
            right -= 1
        return " ".join(wordlist)
```
> - Time: O(N)
> - Space: O(N)
## 55. 右旋字符串
[卡码网](https://kamacoder.com/problempage.php?pid=1065)
### Description
>字符串的右旋转操作是把字符串尾部的若干个字符转移到字符串的前面。给定一个字符串 s 和一个正整数 k，请编写一个函数，将字符串中的后面 k 个字符移到字符串的前面，实现字符串的右旋转操作。
>例如，对于输入字符串 "abcdefg" 和整数 2，函数应该将其转换为 "fgabcde"。
>>**Example 1:**
>>**Input:**
>>2
>>abcdefg
>>**Output:**
>>fgabcde
### Code
>切片秒了
```python
#获取输入的数字k和字    
k = int(input())
s = input()

#通过切片反转第一段和第二段字符串
#注意：python中字符串是不可变的，所以也需要额外空间
s = s[len(s)-k:] + s[:len(s)-k]
print(s)
```
> - Time: O(N)
> - Space: O(N)
## 28. Find the Index of the First Occurrence in a String
[Leetcode Link](https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/description/)-Easy
### Description
>Given two strings needle and haystack, return the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.
>>**Example 1:**
>>**Input:**
>> haystack = "sadbutsad", needle = "sad"
>>**Output:**
>>0
>>**Explanation:**
>>"sad" occurs at index 0 and 6.The first occurrence is at index 0, so we return 0.
### Code
#### Method1 KMP
```python
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        if len(needle) == 0:
            return 0
        next = [0] * len(needle)
        self.getNext(needle,next)
        j = 0
        for i in range(len(haystack)):
            while j > 0 and haystack[i] != needle[j]:
                j = next[j-1]
            if haystack[i] == needle[j]:
                j += 1
            if j == len(needle):
                return i-j+1
        return -1

    def getNext(self,s,next):
        j = 0
        next[0] = 0
        for i in range(1,len(s)):
            while j > 0 and s[j] != s[i]:
                j = next[j-1]
            if s[j] == s[i]:
                j += 1
            next[i] = j
```
> - Time: O(M+N)
> - Space: O(M)
#### Method2 内置函数
```python
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        try:
            return haystack.index(needle)
        except ValueError:
            return -1
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        return haystack.find(needle)
```
> - Time: O(M*N)
> - Space: O(1)
#### Method3 暴力
```python
class Solution(object):
    def strStr(self, haystack, needle):
        m, n = len(haystack), len(needle)
        for i in range(m):
            if haystack[i:i+n] == needle:
                return i
        return -1
```
> - Time: O(M*N)
> - Space: O(1)
## 459. Repeated Substring Pattern
[Leetcode Link](https://leetcode.cn/problems/repeated-substring-pattern/)-Easy
### Description
>Given a string s, check if it can be constructed by taking a substring of it and appending multiple copies of the substring together.
>>**Example 1:**
>>**Input:**
>> s = "abab"
>>**Output:**
>>True
>>**Explanation:**
>>It is the substring "ab" twice.
### Code
#### Method1 KMP
```python
class Solution:
    def repeatedSubstringPattern(self, s: str) -> bool:
        if len(s) == 0:
            return False
        j = 0
        nxt = [0] * len(s)
        self.getNext(s,nxt)
        if nxt[-1] != 0 and len(s)%(len(s)-nxt[-1]) == 0:
            return True
        return False

    def getNext(self,s,nxt):
        j = 0
        nxt[0] = 0
        for i in range(1,len(s)):
            while j >0 and s[i] != s[j]:
                j = nxt[j-1]
            if s[i] == s[j]:
                j += 1
            nxt[i] = j
```
> - Time: O(N)
> - Space: O(N)
#### Method2 移动匹配
```python
class Solution:
    def repeatedSubstringPattern(self, s: str) -> bool:
        n = len(s)
        if n <= 1:
            return False
        ss = s[1:] + s[:-1]
        return ss.find(s) != -1
```
> - Time: O(N)
> - Space: O(1)
## 今日心得
- 学习了KMP算法，来梳理一下。
- KMP算法解决的事字符串匹配的问题，可以使原本暴力的M*N时间复杂度减少到M+N。
- 首先需要实现一个next数组，记录需要匹配的模式串的`最长相等前后缀长度`，比如"aabaaf",它的next数组就是`[0,1,0,1,2,0]`。
- 前后缀的含义分别是不包含字符串最后一个字符/第一个字符的子串，叫前后缀。next数组是记录遍历字符串时前缀与后缀相等的长度。
- 在做字符串匹配的时候，如果碰到匹配不到的时候，就可以用next数组，找到前一个最长相等前后缀长度，从那里进行匹配，不需要一位一位的暴力匹配。
