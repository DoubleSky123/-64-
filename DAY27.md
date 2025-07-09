# DAY 27｜122. Best Time to Buy and Sell Stock II 55. Jump Game 45. Jump Game II 1005. Maximize Sum Of Array After K Negations
## 学习内容
[贪心算法理论基础](https://programmercarl.com/%E8%B4%AA%E5%BF%83%E7%AE%97%E6%B3%95%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html)
## 122. Best Time to Buy and Sell Stock II
[Leetcode Link](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/description/)-Medium
### Description
>You are given an integer array prices where prices[i] is the price of a given stock on the ith day.
>On each day, you may decide to buy and/or sell the stock. You can only hold at most one share of the stock at any time. However, you can buy it then immediately sell it on the same day.
>Find and return the maximum profit you can achieve.
>>**Example 1:**
>>**Input:**
>>prices = [7,1,5,3,6,4]
>>**Output:**
>>7
>>**Explanation:** Buy on day 2 (price = 1) and sell on day 3 (price = 5), profit = 5-1 = 4.
>>Then buy on day 4 (price = 3) and sell on day 5 (price = 6), profit = 6-3 = 3.Total profit is 4 + 3 = 7.
### Code
>和最大子序和一样，把每一天的收益算出来，只累计正收益即可。
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        result = 0
        for i in range(1,len(prices)):
            if prices[i]-prices[i-1] >0:
                result += prices[i]-prices[i-1]
        return result
```
> - Time: O(N)
> - Space: O(1)
## 55. Jump Game
[Leetcode Link](https://leetcode.cn/problems/jump-game/description/)-Medium
### Description
>You are given an integer array nums. You are initially positioned at the array's first index, and each element in the array represents your maximum jump length at that position.
>Return true if you can reach the last index, or false otherwise.
>>**Example 1:**
>>**Input:**
>>nums = [2,3,1,1,4]
>>**Output:**
>>true
>>**Explanation:** You will always arrive at index 3 no matter what. Its maximum jump length is 0, which makes it impossible to reach the last index.
### Code
>遍历时，记录当前位置最大覆盖范围即i+nums[i]，遍历时只遍历到最大范围，看是否覆盖终点。
```python
class Solution:
    def canJump(self, nums: List[int]) -> bool:
        if len(nums) == 1:
            return True
        
        i,cover = 0,0
    
        while i <= cover:
            cover = max(cover,i+nums[i])
            if cover >= len(nums)-1:
                return True
            i += 1
        return False
```
> - Time: O(N)
> - Space: O(1)
## 45. Jump Game II
[Leetcode Link](https://leetcode.cn/problems/jump-game-ii/description/)-Medium
### Description
>You are given a 0-indexed array of integers nums of length n. You are initially positioned at nums[0].
>Each element nums[i] represents the maximum length of a forward jump from index i. In other words, if you are at nums[i], you can jump to any nums[i + j] where:
>0 <= j <= nums[i] and i + j < n
>Return the minimum number of jumps to reach nums[n - 1]. The test cases are generated such that you can reach nums[n - 1].
>>**Example 1:**
>>**Input:**
>>nums = [2,3,1,1,4]
>>**Output:**
>>2
>>**Explanation:** The minimum number of jumps to reach the last index is 2. Jump 1 step from index 0 to 1, then 3 steps to the last index.
### Code
>需要记录当前最大覆盖范围和下一步最大覆盖范围，如果遍历到当前最大范围，步数+1，并且更新当前的最大覆盖范围，当前初始为0。
>遍历到最后为len(nums) - 1，因为如果遍历到当前范围等于len(nums)则步数会+1，会比结果多一步，最后一步不用跳。
```python
class Solution:
     def jump(self, nums):
        cur_distance = 0  # 当前覆盖的最远距离下标
        ans = 0  # 记录走的最大步数
        next_distance = 0  # 下一步覆盖的最远距离下标
        
        for i in range(len(nums) - 1):  # 注意这里是小于len(nums) - 1，这是关键所在
            next_distance = max(nums[i] + i, next_distance)  # 更新下一步覆盖的最远距离下标
            if i == cur_distance:  # 遇到当前覆盖的最远距离下标
                cur_distance = next_distance  # 更新当前覆盖的最远距离下标
                ans += 1
        
        return ans
```
> - Time: O(N)
> - Space: O(1)
## 1005. Maximize Sum Of Array After K Negations
[Leetcode Link](https://leetcode.cn/problems/maximize-sum-of-array-after-k-negations/description/)-Easy
### Description
>Given an integer array nums and an integer k, modify the array in the following way:
>choose an index i and replace nums[i] with -nums[i].You should apply this process exactly k times. You may choose the same index i multiple times.
>Return the largest possible sum of the array after modifying it in this way.
>>**Example 1:**
>>**Input:**
>>nums = [4,2,3], k = 1
>>**Output:**
>>5
>>**Explanation:** Choose index 1 and nums becomes [4,-2,3].
### Code
>先把数组排序，也可以直接按绝对值排序，最小的负数都取反，如果还有k，则取绝对值最小的数一直反转，直到k=0.
```python
class Solution:
    def largestSumAfterKNegations(self, nums: List[int], k: int) -> int:
        nums.sort()
        for i in range(len(nums)):
            if nums[i] < 0 and k >0:
                nums[i] *= -1
                k -= 1
        nums.sort()
        if k%2 == 1:
            nums[0] *= -1
        return sum(nums)
```
> - Time: O(Nlogn)
> - Space: O(1)
## 今日心得
- 买卖股票如果用贪心，代码就会很简单。跳跃游戏的思路需要牢记。
