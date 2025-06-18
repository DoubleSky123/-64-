# DAY 8｜344. Reverse String 541. Reverse String II 54. 替换数字
## 学习内容
[344讲解](https://programmercarl.com/0344.%E5%8F%8D%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)
[541讲解](https://programmercarl.com/0541.%E5%8F%8D%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2II.html)
[替换数字讲解](https://programmercarl.com/kamacoder/0054.%E6%9B%BF%E6%8D%A2%E6%95%B0%E5%AD%97.html)
## 344. Reverse String
[Leetcode Link](https://leetcode.cn/problems/reverse-string/description/)-Easy
### Description
>Write a function that reverses a string. The input string is given as an array of characters s.
>You must do this by modifying the input array in-place with O(1) extra memory.
>>**Example 1:**
>>**Input:**
>>s = ["h","e","l","l","o"]
>>**Output:**
>>["o","l","l","e","h"]
### Code
>双指针秒了，python还可以用reverse()函数和切片。
```python
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        left = 0
        right = len(s)-1
        while left < right:
            s[left],s[right] = s[right],s[left]
            left += 1
            right -= 1
        return s 
        ##方法二：原地反转
        ##s.reverse()
        ##方法三：切片反转
        ##s[:] = s[::-1]
        ##无需return
```
> - Time: O(N)
> - Space: O(1)
## 541. Reverse String II
[Leetcode Link](https://leetcode.cn/problems/reverse-string-ii/description/)-Easy
### Description
>Given a string s and an integer k, reverse the first k characters for every 2k characters counting from the start of the string.
>If there are fewer than k characters left, reverse all of them.
>If there are less than 2k but greater than or equal to k characters, then reverse the first k characters and leave the other as original.
>>**Example 1:**
>>**Input:**
>>s = "abcdefg", k = 2
>>**Output:**
>>"bacdfeg"
### Code
>题意稍微有点难理解，每2k个字符，反转前k个，最后不足k个都反转。
>切片秒了，双指针也可以，和上题差不多。
```python
class Solution:
    def reverseStr(self, s: str, k: int) -> str:
        p = 0
        while p < len(s):
            p2 = p+k
            s = s[:p] + s[p:p2][::-1] + s[p2:]
            p += 2*k
        
        return s
```
> - Time: O(N)
> - Space: O(1)
## 54. 替换数字
[卡码网](https://kamacoder.com/problempage.php?pid=1064)
### Description
>给定一个字符串 s，它包含小写字母和数字字符，请编写一个函数，将字符串中的字母字符保持不变，而将每个数字字符替换为number。 例如，对于输入字符串 "a1b2c3"，函数应该将其转换为 "anumberbnumbercnumber"。
>>**Example 1:**
>>**Input:**
>>a1b2c3
>>**Output:**
>>anumberbnumbercnumber
### Code
```python
class Solution(object):
    def subsitute_numbers(self, s):
        """
        :type s: str
        :rtype: str
        """
        count = sum(1 for char in s if char.isdigit()) 
        expand_len = len(s) + (count * 5)  
        res = [''] * expand_len

        new_index = expand_len - 1 # 指向扩充后字符串末尾
        old_index = len(s) - 1 # 指向原字符串末尾
        
        while old_index >= 0: # 从后往前， 遇到数字替换成“number”
            if s[old_index].isdigit():
                res[new_index-5:new_index+1] = "number"
                new_index -= 6
            else:
                res[new_index] = s[old_index]
                new_index -= 1
            old_index -= 1
        
        return "".join(res)

if __name__ == "__main__":
    solution = Solution()

    while True:
        try:
            s = input()
            result = solution.subsitute_numbers(s)
            print(result)
        except EOFError:
            break
```
> - Time: O(N)
> - Space: O(1)
## 今日心得
- 秒了
- python中字符串可以善用切片进行反转操作。
