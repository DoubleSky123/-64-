# DAY 42｜188. Best Time to Buy and Sell Stock IV 309. Best Time to Buy and Sell Stock with Cooldown 714. Best Time to Buy and Sell Stock with Transaction Fee
## 学习内容
[股票交易理论基础](https://programmercarl.com/%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92-%E8%82%A1%E7%A5%A8%E9%97%AE%E9%A2%98%E6%80%BB%E7%BB%93%E7%AF%87.html)
## 188. Best Time to Buy and Sell Stock IV
[Leetcode Link](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-iv/description/)-Hard
### Description
>You are given an integer array prices where prices[i] is the price of a given stock on the ith day, and an integer k.
>Find the maximum profit you can achieve. You may complete at most k transactions: i.e. you may buy at most k times and sell at most k times.
>Note: You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).
>>**Example 1:**
>>**Input:**
>>k = 2, prices = [2,4,1]
>>**Output:**
>>2
>>**Explanation:**
>>Buy on day 1 (price = 2) and sell on day 2 (price = 4), profit = 4-2 = 2.
### Code
>至多买卖k次，将[j]与k关联：0表示不操作，第一次买卖1，2；第二次买卖3，4，第三次买卖5，6. 买就是1，3，5，7...
>初始化2*k都为-prices[0]
```python
class Solution:
    def maxProfit(self, k: int, prices: List[int]) -> int:
        dp = [[0]*(2*k+1) for _ in range(len(prices))]

        for j in range(1,2*k,2):
            dp[0][j] = -prices[0]

        for i in range(1,len(prices)):
            for j in range(0,2*k-1,2):
                dp[i][j+1] = max(dp[i-1][j+1], dp[i-1][j] - prices[i])
                dp[i][j+2] = max(dp[i-1][j+2], dp[i-1][j+1] + prices[i])
        return dp[-1][2*k]
```
> - Time: O(Nk)
> - Space: O(Nk)
## 309. Best Time to Buy and Sell Stock with Cooldown
[Leetcode Link](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-with-cooldown/description/)-Medium
### Description
>You are given an array prices where prices[i] is the price of a given stock on the ith day.
>Find the maximum profit you can achieve. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times) with the following restrictions:
>After you sell your stock, you cannot buy stock on the next day (i.e., cooldown one day).
>Note: You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).
>>**Example 1:**
>>**Input:**
>>prices = [1,2,3,0,2]
>>**Output:**
>>3
>>**Explanation:**
>>transactions = [buy, sell, cooldown, buy, sell]
### Code
>包含这几个状态，0: 持有股票，1: 不持有股票，2: 当天卖出股票，3: 冷冻期。
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        dp = [[0]*4 for _ in range(len(prices))]
        ## 0: 持有股票，1: 不持有股票，2: 当天卖出股票，3: 冷冻期
        dp[0][0] = -prices[0]
        dp[0][1] = 0
        dp[0][2] = 0
        dp[0][3] = 0

        for i in range(1,len(prices)):
            dp[i][0] = max(dp[i-1][0],dp[i-1][3]-prices[i],dp[i-1][1]-prices[i])
            dp[i][1] = max(dp[i-1][1],dp[i-1][3])
            dp[i][2] = dp[i-1][0]+prices[i]
            dp[i][3] = dp[i-1][2]

        return max(dp[-1][1],dp[-1][2],dp[-1][3])
```
> - Time: O(N)
> - Space: O(N)
## 714. Best Time to Buy and Sell Stock with Transaction Fee
[Leetcode Link](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/description/)-Medium
### Description
>You are given an array prices where prices[i] is the price of a given stock on the ith day, and an integer fee representing a transaction fee.
>Find the maximum profit you can achieve. You may complete as many transactions as you like, but you need to pay the transaction fee for each transaction.
>Note: You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again). The transaction fee is only charged once for each stock purchase and sale.
>>**Example 1:**
>>**Input:**
>>prices = [1,3,2,8,4,9], fee = 2
>>**Output:**
>>8
>>**Explanation:**
>>The maximum profit can be achieved by:- Buying at prices[0] = 1 - Selling at prices[3] = 8 - Buying at prices[4] = 4 - Selling at prices[5] = 9 The total profit is ((8 - 1) - 2) + ((9 - 4) - 2) = 8.
### Code
>在基础股票交易的基础上，-fee
```python
class Solution:
    def maxProfit(self, prices: List[int], fee: int) -> int:
        dp = [[0]*2 for _ in range(len(prices))]
        ## 0 = 持有/1 = 不持有
        dp[0][0] = -prices[0]
        dp[0][1] = 0

        for i in range(1,len(prices)):
            dp[i][0] = max(dp[i-1][0],dp[i-1][1]-prices[i])
            dp[i][1] = max(dp[i-1][1],dp[i-1][0]+prices[i]-fee)
        
        return dp[-1][1]
```
> - Time: O(N)
> - Space: O(N*5)
## 今日心得
- 股票问题需要用二维数组，[i]表示第i天，[j]表示操作股票的状态，根据题意来确定，有多少种买卖的状态。
