# DAY 45｜115. Distinct Subsequences 583. Delete Operation for Two Strings 72. Edit Distance
## 学习内容
[编辑距离理论基础](https://programmercarl.com/%E4%B8%BA%E4%BA%86%E7%BB%9D%E6%9D%80%E7%BC%96%E8%BE%91%E8%B7%9D%E7%A6%BB%EF%BC%8C%E5%8D%A1%E5%B0%94%E5%81%9A%E4%BA%86%E4%B8%89%E6%AD%A5%E9%93%BA%E5%9E%AB.html)
## 115. Distinct Subsequences
[Leetcode Link](https://leetcode.cn/problems/distinct-subsequences/description/)-Hard
### Description
>Given two strings s and t, return the number of distinct subsequences of s which equals t.The test cases are generated so that the answer fits on a 32-bit signed integer.
>>**Example 1:**
>>**Input:**
>>s = "rabbbit", t = "rabbit"
>>**Output:**
>>3
>>**Explanation:**
>>As shown below, there are 3 ways you can generate "rabbit" from s. rabbbit rabbbit rabbbit
### Code
> dp[i][j]：以i-1为结尾的s子序列中出现以j-1为结尾的t的个数为dp[i][j]。
> i-1 == j-1时，可以同时删除这两个字母，统计前面以i-2结尾的dp[i][j]，也可以只不考虑i-1，删除i-1.判断子序列个数所以是dp[i-1][j]，只删parent字符串。
```python
class Solution:
    def numDistinct(self, s: str, t: str) -> int:
        dp = [[0]*(len(t)+1) for _ in range(len(s)+1)]

        for i in range(len(s)+1):
            dp[i][0] = 1

        for i in range(1,len(s)+1):
            for j in range(1,len(t)+1):
                if s[i-1] == t[j-1]:
                    dp[i][j] = dp[i-1][j-1] + dp[i-1][j] ## i-1删或不删情况相加
                else:
                    dp[i][j] = dp[i-1][j]
        return dp[-1][-1]
```
> - Time: O(N^2)
> - Space: O(N^2)
## 583. Delete Operation for Two Strings
[Leetcode Link](https://leetcode.cn/problems/delete-operation-for-two-strings/description/)-Medium
### Description
>Given two strings word1 and word2, return the minimum number of steps required to make word1 and word2 the same.In one step, you can delete exactly one character in either string.
>>**Example 1:**
>>**Input:**
>>word1 = "sea", word2 = "eat"
>>**Output:**
>>2
>>**Explanation:**
>>You need one step to make "sea" to "ea" and another step to make "eat" to "ea".
### Code
>求最长公共子序列长度，然后两个字符串长度减掉最长公共子序列长度，就是最少要删的长度。
```python
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        dp = [[0]*(len(word2)+1) for _ in range(len(word1)+1)]

        for i in range(1,len(word1)+1):
            for j in range(1,len(word2)+1):
                if word1[i-1]== word2[j-1]:
                    dp[i][j] = dp[i-1][j-1]+1
                else:
                    dp[i][j] = max(dp[i-1][j],dp[i][j-1])
        return len(word1)+len(word2)-2*dp[-1][-1]
```
> - Time: O(MN)
> - Space: O(MN)
## 72. Edit Distance
[Leetcode Link](https://leetcode.cn/problems/edit-distance/description/)-Medium
### Description
>Given two strings word1 and word2, return the minimum number of operations required to convert word1 to word2.
>You have the following three operations permitted on a word: Insert a character Delete a character Replace a character
>>**Example 1:**
>>**Input:**
>>word1 = "horse", word2 = "ros"
>>**Output:**
>>3
>>**Explanation:**
>>horse -> rorse (replace 'h' with 'r') rorse -> rose (remove 'r') rose -> ros (remove 'e')
### Code
>初始化，[i][0],[0][j]即当一个字符串为空时，另一个字符串需要删掉它长度的次数才能相等，所以初始化为i和j。
>当i-1==j-1时，不操作，即dp[i][j] = dp[i-1][j-1]；不相等时，要么删除i-1要么删除j-1，要么替换一个，替换是为了两个元素相同，相同是 dp[i-1][j-1]，基础上+1.三种操作取最小。
```python
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        dp = [[0]*(len(word2)+1) for _ in range(len(word1)+1)]

        for i in range(len(word1)+1):
            dp[i][0] = i
        for j in range(len(word2)+1):
            dp[0][j] = j
        
        for i in range(1,len(word1)+1):
            for j in range(1,len(word2)+1):
                if word1[i-1] == word2[j-1]:
                    dp[i][j] = dp[i-1][j-1]
                else:
                    dp[i][j] = min(dp[i-1][j]+1,dp[i][j-1]+1,dp[i-1][j-1]+1)
        return dp[-1][-1]
```
> - Time: O(MN)
> - Space: O(MN)
## 今日心得
- 编辑距离问题很难理解，尤其是115和72，需要画图和画表格多理解。
