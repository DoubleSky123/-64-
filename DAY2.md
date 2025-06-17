# DAY 2｜209. Minimum Size Subarray Sum 59. Spiral Matrix II 区间和 开发商购买土地
## 学习内容
[209讲解](https://programmercarl.com/0209.%E9%95%BF%E5%BA%A6%E6%9C%80%E5%B0%8F%E7%9A%84%E5%AD%90%E6%95%B0%E7%BB%84.html)
[59讲解](https://programmercarl.com/0059.%E8%9E%BA%E6%97%8B%E7%9F%A9%E9%98%B5II.html)
[区间和讲解](https://www.programmercarl.com/kamacoder/0058.%E5%8C%BA%E9%97%B4%E5%92%8C.html)
[开发商购买土地讲解](https://www.programmercarl.com/kamacoder/0044.%E5%BC%80%E5%8F%91%E5%95%86%E8%B4%AD%E4%B9%B0%E5%9C%9F%E5%9C%B0.html)
## 209. Minimum Size Subarray Sum
[Leetcode Link](https://leetcode.cn/problems/minimum-size-subarray-sum/description/)-Medium
### Description
>Given an array of positive integers nums and a positive integer target,
>return the minimal length of a subarray whose sum is greater than or equal to target.
>If there is no such subarray, return 0 instead.
>>**Example 1:**
>>**Input:** target = 7, nums = `[2,3,1,2,4,3]`
>>**Output:** 2
>>**Explanation:** The subarray [4,3] has the minimal length under the problem constraint.
### Code
#### Method1 滑动窗口
```python
class Solution:
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        if sum(nums) < target:
            return 0
        
        result = float('inf')
        sub_sum = 0
        i = 0 #起始指针，窗口左边界
        for j in range(len(nums)):
            sub_sum += nums[j]
            while sub_sum >= target:
                subL = j - i + 1
                result = min(subL,result)
                sub_sum = sub_sum - nums[i]
                i += 1 #起始指针右移
        
        return result
```
> - Time: O(N)
> - Space: O(1)
#### Method2 暴力
```python
class Solution:
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        l = len(nums)
        min_size = float('inf')
        for i in range(l):
            sub_sum = 0
            for j in range(i,l):
                sub_sum += nums[j]
                if sub_sum >= target:
                    min_size = min(min_size,j-i+1)
        return min_size if min_size != float('inf') else 0
```
> - Time: O(N**2)
> - Space: O(1)
## 904. Fruit Into Baskets（拓展）
[Leetcode Link](https://leetcode.cn/problems/fruit-into-baskets/description/)-Medium
### Description
>You are visiting a farm that has a single row of fruit trees arranged from left to right.
>The trees are represented by an integer array fruits where fruits[i] is the type of fruit the ith tree produces.
>You want to collect as much fruit as possible. However, the owner has some strict rules that you must follow:
>You only have two baskets, and each basket can only hold a single type of fruit. There is no limit on the amount of fruit each basket can hold.
>Starting from any tree of your choice, you must pick exactly one fruit from every tree (including the start tree) while moving to the right.
>The picked fruits must fit in one of your baskets.
>Once you reach a tree with fruit that cannot fit in your baskets, you must stop.
>Given the integer array fruits, return the maximum number of fruits you can pick.
>>**Example 1:**
>>**Input:** fruits = `[1,2,3,2,2]`
>>**Output:** 4
>>**Explanation:** We can pick from trees [2,3,2,2].If we had started at the first tree, we would only pick from trees [1,2].
### Code
>滑动窗口+哈希结合，首次接触`Counter()`用来记录果树出现频率。这道题可以转化为窗口最多只能含有两个数，求最长窗口长度。
>当窗口内含有三个数的时候，左边界开始移动，并且果树值-1，如果果树值为0则将其移出。
```python
class Solution:
    def totalFruit(self, fruits: List[int]) -> int:
        cnt = Counter()

        left = ans = 0
        for right, x in enumerate(fruits):
            cnt[x] += 1
            while len(cnt) > 2:
                cnt[fruits[left]] -= 1
                if cnt[fruits[left]] == 0:
                    cnt.pop(fruits[left])
                left += 1
            ans = max(ans, right - left + 1)
        
        return ans
```
> - Time: O(N)
> - Space: O(1)
## 76. Minimum Window Substring（拓展）
[Leetcode Link](https://leetcode.cn/problems/minimum-window-substring/description/)-Hard
### Description
>Given two strings s and t of lengths m and n respectively,
>return the minimum window substring of s such that every character in t (including duplicates) is included in the window.
>If there is no such substring, return the empty string "".
>The testcases will be generated such that the answer is unique.
>>**Example 1:**
>>**Input: s** = `"ADOBECODEBANC"`, t = `"ABC"`
>>**Output:** "BANC"
>>**Explanation:** The minimum window substring "BANC" includes 'A', 'B', and 'C' from string t.
### Code
>滑动窗口，右边界先保证可以完全包括目标字符串，然后开始移动左边界。如果左边界移动到不包含了，再移动右边界到包含字符串，再移动左边界。不断更新最小边界。
>并且学到了可以用`Counter()`直接比大小
```python
class Solution:
    def minWindow(self, s: str, t: str) -> str:
        ans_left, ans_right = -1,len(s)
        cnt_s = Counter()
        cnt_t = Counter(t)
        left = 0
        for right, x in enumerate(s):
            cnt_s[x] += 1
            while cnt_s >= cnt_t:
                if right - left < ans_right - ans_left:
                    ans_left, ans_right = left, right
                cnt_s[s[left]] -= 1 
                left += 1
         
        return "" if ans_left < 0 else s[ans_left: ans_right + 1]
```
> - Time: O(N)
> - Space: O(N)
## 59. Spiral Matrix II
[Leetcode Link](https://leetcode.cn/problems/spiral-matrix-ii/)-Medium
### Description
>Given a positive integer n, generate an n x n matrix filled with elements from 1 to n2 in spiral order.
>>**Example 1:**
>>**Input:** n = 3
>>**Output:** `[[1,2,3],[8,9,4],[7,6,5]]`
### Code
>循环不变，左闭右开。这是一个不变的循环，只要每次处理这个矩阵的四条边的边界都保持左闭右开即可。如果n是奇数，需要处理中间值。最外面遍历循环次数用来处理边界offset，每次向内循环边界都会变小。
```python
class Solution:
    def generateMatrix(self, n: int) -> List[List[int]]:
        result = [[0] * n for _ in range(n)]
        startx, starty = 0, 0             
        loop = n // 2       
        count = 1  

        for offset in range(1,loop+1):
            for i in range(starty,n-offset):
                result[startx][i] = count
                count += 1
            for i in range(startx,n-offset):
                result[i][n-offset] = count
                count += 1
            for i in range(n-offset,starty,-1):
                result[n-offset][i] = count
                count += 1
            for i in range(n-offset,startx,-1):
                result[i][startx] = count
                count += 1
            startx += 1
            starty += 1
        if n % 2 == 1:
            result[startx][starty] = count

        return result
```
> - Time: O(N**2)
> - Space: O(N**2)
## 54. Spiral Matrix（拓展）
[Leetcode Link](https://leetcode.cn/problems/spiral-matrix/description/)-Medium
### Description
>Given an m x n matrix, return all elements of the matrix in spiral order.
>>**Example 1:**
>>**Input:** matrix = `[[1,2,3,4],[5,6,7,8],[9,10,11,12]]`
>>**Output:** `[1,2,3,4,8,12,11,10,9,5,6,7]`
### Code
>做了上一题之后，延续了上一题的做法，这道题要注意，当mn不相等是个长方形矩阵的时候，长的边需要单独处理。
```python
class Solution:
    def spiralOrder(self, matrix: List[List[int]]) -> List[int]:
        if not matrix:
            return ""
        result = []
        row = len(matrix)
        column = len(matrix[0])
        startx,starty = 0,0
        loop = min(row, column) // 2

        for offset in range(1,loop+1):
            for i in range(starty, column - offset):
                result.append(matrix[startx][i])
            # 从上到下
            for i in range(startx, row - offset):
                result.append(matrix[i][column - offset])
            # 从右到左
            for i in range(column - offset, starty, -1):
                result.append(matrix[row - offset][i])
            # 从下到上
            for i in range(row - offset, startx, -1):
                result.append(matrix[i][starty])
            startx += 1
            starty += 1

        if min(row, column) % 2 == 1:
            if row <= column:  # 剩下一行
                for i in range(starty, column - starty):
                    result.append(matrix[startx][i])
            else:  # 剩下一列
                for i in range(startx, row - startx):
                    result.append(matrix[i][starty])
        
        return result
```
> - Time: O(MN)
> - Space: O(MN)
## 58. 区间和
[卡玛网](https://kamacoder.com/problempage.php?pid=1070)-ACM
### Description
>给定一个整数数组 Array，请计算该数组在每个指定区间内元素的总和。
>>**Explanation:**
>>**Input:** 第一行输入为整数数组 Array 的长度 n，接下来 n 行，每行一个整数，表示数组的元素。随后的输入为需要计算总和的区间下标：a，b （b > = a），直至文件结束。
>>**Output:** 输出每个指定区间内元素的总和。
### Code
>前缀和presum。定义一个前缀和数组，遍历数组时，计算每一项之前的和插入到新数组。要计算两个下标之间的和，就可以直接用前缀和数组相减。
```python
import sys
input = sys.stdin.read

def main():
    data = input().split()
    index = 0
    n = int(data[index])
    index += 1
    vec = []
    for i in range(n):
        vec.append(int(data[index + i]))
    index += n

    p = [0] * n
    presum = 0
    for i in range(n):
        presum += vec[i]
        p[i] = presum

    results = []
    while index < len(data):
        a = int(data[index])
        b = int(data[index + 1])
        index += 2

        if a == 0:
            sum_value = p[b]
        else:
            sum_value = p[b] - p[a - 1]

        results.append(sum_value)

    for result in results:
        print(result)

if __name__ == "__main__":
    main()
```
> - Time: O(N)
> - Space: O(1)
## 44. 开发商购买土地
[卡玛网](https://kamacoder.com/problempage.php?pid=1044)-ACM
### Description
>在一个城市区域内，被划分成了n * m个连续的区块，每个区块都拥有不同的权值，代表着其土地价值。目前，有两家开发公司，A 公司和 B 公司，希望购买这个城市区域的土地。
>现在，需要将这个城市区域的所有区块分配给 A 公司和 B 公司。
>然而，由于城市规划的限制，只允许将区域按横向或纵向划分成两个子区域，而且每个子区域都必须包含一个或多个区块。 为了确保公平竞争，你需要找到一种分配方式，使得 A 公司和 B 公司各自的子区域内的土地总价值之差最小。
>注意：区块不可再分。
>>**Explanation:**
>>**Input:** 第一行输入两个正整数，代表 n 和 m。接下来的 n 行，每行输出 m 个正整数。
>>**Output:** 请输出一个整数，代表两个子区域内土地总价值之间的最小差距。
### Code
前缀和presum
```python
import sys
input = sys.stdin.read

def main():
    data = input().split()

    idx = 0
    n = int(data[idx])
    idx += 1
    m = int(data[idx])
    idx += 1
    sum = 0
    vec = []
    for i in range(n):
        row = []
        for j in range(m):
            num = int(data[idx])
            idx += 1
            row.append(num)
            sum += num
        vec.append(row)

    # 统计横向
    horizontal = [0] * n
    for i in range(n):
        for j in range(m):
            horizontal[i] += vec[i][j]

    # 统计纵向
    vertical = [0] * m
    for j in range(m):
        for i in range(n):
            vertical[j] += vec[i][j]

    result = float('inf')
    horizontalCut = 0
    for i in range(n):
        horizontalCut += horizontal[i]
        result = min(result, abs(sum - 2 * horizontalCut))

    verticalCut = 0
    for j in range(m):
        verticalCut += vertical[j]
        result = min(result, abs(sum - 2 * verticalCut))

    print(result)

if __name__ == "__main__":
    main()
```
> - Time: O(MN)
> - Space: O(1)
## 今日心得
- 今天学习了三个新的算法：滑动窗口，前缀和，螺旋遍历矩阵。感觉在做数组题目的时候都非常有用，并且还做了对应的拓展，让我对这三个算法加深印象，因为平时接触的少，很难想到。
- **滑动窗口**是双指针的变形，要先移右边界，再移左边界；
>当题目涉及连续区间（子数组/子串）且需要动态维护某种“状态”（和、频率、数量）时，首先考虑滑动窗口。
>`常见适用场景：`连续子数组 / 子串,明确要求子数组或子串是连续的，如“最长”、“最短”、“包含”之类的。
>数组 / 字符串中的某种“窗口状态”,子数组的和、乘积、字符频率、是否包含所有目标字符等。
>`常见关键词信号：`“子数组/子串的和为/大于/小于……”“最小长度 / 最长区间”“连续”“满足某个条件的子串/子数组”
- **前缀和**是遍历数组时把每一项到之前的和算出来并且创建一个新数组，如果要求某个下标区间的和就可以直接相减。
- **螺旋遍历**矩阵是螺旋方式遍历矩阵的时候，要注意处理每一条边的边界保持相同，左闭右开。最外层是螺旋圈数，并且可以把它用作减去的边界。最后注意奇数边的单独处理。




