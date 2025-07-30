# DAY 41｜121. Best Time to Buy and Sell Stock 122. Best Time to Buy and Sell Stock II 123. Best Time to Buy and Sell Stock III
## 学习内容
[股票交易理论基础](https://programmercarl.com/%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92-%E8%82%A1%E7%A5%A8%E9%97%AE%E9%A2%98%E6%80%BB%E7%BB%93%E7%AF%87.html)
## 121. Best Time to Buy and Sell Stock
[Leetcode Link](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock/description/)-Easy
### Description
>You are given an array prices where prices[i] is the price of a given stock on the ith day.
>You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock.
>Return the maximum profit you can achieve from this transaction. If you cannot achieve any profit, return 0.
>>**Example 1:**
>>**Input:**
>>prices = [7,1,5,3,6,4]
>>**Output:**
>>5
>>**Explanation:**
>>Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5. Note that buying on day 2 and selling on day 1 is not allowed because you must buy before you sell.
### Code
>0：持有股票，1：不持有股票，注意初始化dp[0][0]=第0天持有这只股票即买入，所以是prices[0]。
>根据i-1天的持有和非持有的状态转移，求出最大dp[i][j]
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        dp = [[0] * 2 for _ in range(len(prices))]
        dp[0][0] = -prices[0]
        dp[0][1] = 0

        for i in range(1,len(prices)):
            dp[i][0] = max(dp[i-1][0],-prices[i])
            dp[i][1] = max(dp[i-1][1],prices[i]+dp[i-1][0])
        
        return dp[-1][1]
```
> - Time: O(N)
> - Space: O(N)
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
>>**Explanation:**
>>Buy on day 2 (price = 1) and sell on day 3 (price = 5), profit = 5-1 = 4. Then buy on day 4 (price = 3) and sell on day 5 (price = 6), profit = 6-3 = 3. Total profit is 4 + 3 = 7.
### Code
>和上题的区别是一次和多次，在持有状态下，上次不持有改成dp[i-1][1]-prices[i]即可。
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        dp = [[0]*2 for _ in range(len(prices))]

        dp[0][0] = -prices[0]
        dp[0][1] = 0

        for i in range(1,len(prices)):
            dp[i][0] = max(dp[i-1][0],dp[i-1][1]-prices[i])
            dp[i][1] = max(dp[i-1][1],prices[i]+dp[i-1][0])
        
        return dp[-1][1]
```
> - Time: O(N)
> - Space: O(N)
## 123. Best Time to Buy and Sell Stock III
[Leetcode Link](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-iii/description/)-Hard
### Description
>You are given an array prices where prices[i] is the price of a given stock on the ith day.
>Find the maximum profit you can achieve. You may complete at most two transactions.
>Note: You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).
>>**Example 1:**
>>**Input:**
>>prices = [3,3,5,0,0,3,1,4]
>>**Output:**
>>6
>>**Explanation:**
>>Buy on day 4 (price = 0) and sell on day 6 (price = 3), profit = 3-0 = 3. Then buy on day 7 (price = 1) and sell on day 8 (price = 4), profit = 4-1 = 3.
### Code
>至多买卖2次，把持有和不持有的状态增加到第一次买入，第一次卖出，和第二次买入，第二吃卖出。
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        '''
        0:不操作
        1:第一次买入
        2.第一次卖出
        3.第二次买入
        4.第二次卖出
        '''
        if len(prices) == 0:
            return 0
        dp = [[0]*5 for _ in range(len(prices))]
        dp[0][1] = -prices[0]
        dp[0][3] = -prices[0]

        for i in range(1,len(prices)):
            dp[i][0] = dp[i-1][0]
            dp[i][1] = max(dp[i-1][1], dp[i-1][0] - prices[i])
            dp[i][2] = max(dp[i-1][2], dp[i-1][1] + prices[i])
            dp[i][3] = max(dp[i-1][3], dp[i-1][2] - prices[i])
            dp[i][4] = max(dp[i-1][4], dp[i-1][3] + prices[i]) 

        return dp[-1][4]
```
> - Time: O(N)
> - Space: O(N*5)
## 今日心得
- 股票问题需要用二维数组，[i]表示第i天，[j]表示操作股票的状态，根据题意来确定，有多少种买卖的状态。
