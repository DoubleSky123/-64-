# DAY 46｜647. Palindromic Substrings 516. Longest Palindromic Subsequence
## 学习内容
[回文串理论基础](https://programmercarl.com/0647.%E5%9B%9E%E6%96%87%E5%AD%90%E4%B8%B2.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)
## 647. Palindromic Substrings
[Leetcode Link](https://leetcode.cn/problems/palindromic-substrings/description/)-Medium
### Description
>Given a string s, return the number of palindromic substrings in it. A string is a palindrome when it reads the same backward as forward. A substring is a contiguous sequence of characters within the string.
>>**Example 1:**
>>**Input:**
>>s = "abc"
>>**Output:**
>>3
>>**Explanation:**
>>Three palindromic strings: "a", "b", "c".
### Code
>i,j作为字符串的两端，判断[i:j]这个字符串是否为回文串。
>初始化是都为False。
>如果相等，且i到j的长度小于等于1的话，result+=1，如果长度大于1，继续向内判断i+1:j-1是否为true。
```python
class Solution:
    def countSubstrings(self, s: str) -> int:
        '''
        dp[i][j] 判断以i，j为两端的子串是不是回文串
        '''
        dp = [[False] * len(s) for _ in range(len(s))]
        result = 0
        for i in range(len(s)-1,-1,-1):
            for j in range(i,len(s)):
                if s[i] == s[j]:
                    if j-i <= 1:
                        result += 1
                        dp[i][j] = True
                    elif dp[i+1][j-1]:
                        result += 1
                        dp[i][j] = True 
        return result
```
> - Time: O(N^2)
> - Space: O(N^2)
## 516. Longest Palindromic Subsequence
[Leetcode Link](https://leetcode.cn/problems/longest-palindromic-subsequence/description/)-Medium
### Description
>Given a string s, find the longest palindromic subsequence's length in s. A subsequence is a sequence that can be derived from another sequence by deleting some or no elements without changing the order of the remaining elements.
>>**Example 1:**
>>**Input:**
>>s = "bbbab"
>>**Output:**
>>4
>>**Explanation:**
>>One possible longest palindromic subsequence is "bbbb".
### Code
>dp[i][j]：字符串s在[i, j]范围内最长的回文子序列的长度为dp[i][j]。
>看i和j是否相同，如果相同，那就在dp[i+1][j-1] +2，如果不同，取dp[i+1][j],dp[i][j-1]最大值。
>初始化：当i与j相同，i和j一直向内移动移动到指向同一个元素，即：一个字符的回文子序列长度就是1。
```python
class Solution:
    def longestPalindromeSubseq(self, s: str) -> int:
        dp = [[0]* len(s) for _ in range(len(s))]

        for i in range(len(s)):
            dp[i][i] = 1
        for i in range(len(s)-1,-1,-1):
            for j in range(i+1,len(s)):
                if s[i] == s[j]:
                    dp[i][j] = dp[i+1][j-1] +2
                else:
                    dp[i][j] = max(dp[i+1][j],dp[i][j-1])
        return dp[0][-1]
```
> - Time: O(N^2)
> - Space: O(N^2)
## 今日心得
- 最后的回文串题目，dp数组是i到j范围的字符串，即i和j为字符串两端。动规完结！
