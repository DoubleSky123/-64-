# DAY 28｜134. Gas Station 135. Candy 860. Lemonade Change 406. Queue Reconstruction by Height
## 学习内容
[贪心算法理论基础](https://programmercarl.com/%E8%B4%AA%E5%BF%83%E7%AE%97%E6%B3%95%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html)
## 134. Gas Station
[Leetcode Link](https://leetcode.cn/problems/gas-station/description/)-Medium
### Description
>There are n gas stations along a circular route, where the amount of gas at the ith station is gas[i].
>You have a car with an unlimited gas tank and it costs cost[i] of gas to travel from the ith station to its next (i + 1)th station. You begin the journey with an empty tank at one of the gas stations.
>Given two integer arrays gas and cost, return the starting gas station's index if you can travel around the circuit once in the clockwise direction, otherwise return -1.
>If there exists a solution, it is guaranteed to be unique.
>>**Example 1:**
>>**Input:**
>>gas = [1,2,3,4,5], cost = [3,4,5,1,2]
>>**Output:**
>>3
>>**Explanation:** Start at station 3 (index 3) and fill up with 4 unit of gas. Your tank = 0 + 4 = 4 Travel to station 4. Your tank = 4 - 1 + 5 = 8
>>Travel to station 0. Your tank = 8 - 2 + 1 = 7 Travel to station 1. Your tank = 7 - 3 + 2 = 6 Travel to station 2. Your tank = 6 - 4 + 3 = 5
>>Travel to station 3. The cost is 5. Your gas is just enough to travel back to station 3. Therefore, return 3 as the starting index.
### Code
>遍历数组，累加每个加油站的油-油耗，如果小于0，就把下一个点设为起点。
>累加总的油-油耗，如果小于0，说明开不到，如果大于等于0，说明一定有一个起点。
```python
class Solution:
    def canCompleteCircuit(self, gas: List[int], cost: List[int]) -> int:
        curSum = 0
        totalSum = 0
        start = 0
        for i in range(len(gas)):
            curSum += gas[i]-cost[i]
            totalSum += gas[i]-cost[i]
            if curSum <0:
                start = i+1
                curSum = 0
        if totalSum <0:
            return -1
        return start
```
> - Time: O(N)
> - Space: O(1)
## 135. Candy
[Leetcode Link](https://leetcode.cn/problems/candy/description/)-Hard
### Description
>There are n children standing in a line. Each child is assigned a rating value given in the integer array ratings.
>You are giving candies to these children subjected to the following requirements:
>Each child must have at least one candy.Children with a higher rating get more candies than their neighbors.
>Return the minimum number of candies you need to have to distribute the candies to the children.
>>**Example 1:**
>>**Input:**
>>ratings = [1,0,2]
>>**Output:**
>>5
>>**Explanation:** You can allocate to the first, second and third child with 2, 1, 2 candies respectively.
### Code
>这道题需要比较两边，但不能同时比较，可以一边一边比较。
>先从左边比较，记录一个糖数，再从右边比较，取最大的糖数。和product except itself很像。
```python
class Solution:
    def candy(self, ratings: List[int]) -> int:
        candy_num = [1]*len(ratings)
        for i in range(1,len(ratings)):
            if ratings[i]>ratings[i-1]:
                candy_num[i] = candy_num[i-1] +1
        for i in range(len(ratings)-2,-1,-1):
            if ratings[i]>ratings[i+1]:
                candy_num[i] = max(candy_num[i],candy_num[i+1]+1)
        return sum(candy_num)
```
> - Time: O(N)
> - Space: O(N)
## 860. Lemonade Change
[Leetcode Link](https://leetcode.cn/problems/lemonade-change/description/)-Easy
### Description
>At a lemonade stand, each lemonade costs $5. Customers are standing in a queue to buy from you and order one at a time (in the order specified by bills).
>Each customer will only buy one lemonade and pay with either a $5, $10, or $20 bill. You must provide the correct change to each customer so that the net transaction is that the customer pays $5.
>Note that you do not have any change in hand at first.
>Given an integer array bills where bills[i] is the bill the ith customer pays, return true if you can provide every customer with the correct change, or false otherwise.
>>**Example 1:**
>>**Input:**
>>bills = [5,5,5,10,20]
>>**Output:**
>>true
>>**Explanation:** From the first 3 customers, we collect three $5 bills in order.From the fourth customer, we collect a $10 bill and give back a $5.
>>From the fifth customer, we give a $10 bill and a $5 bill. Since all customers got correct change, we output true.
### Code
>记录5，10，20的数量即可，每一种情况都用if进行判断。
```python
class Solution:
    def lemonadeChange(self, bills: List[int]) -> bool:
        if bills[0] != 5:
            return False
        
        change = [0] * 2
        for i in bills:
            if i == 5:
                change[0] += 1
            elif i == 10:
                change[0] -= 1
                change[1] += 1
                if change[0] <0:
                    return False
            else:
                if change[1]>0:
                    change[0] -= 1
                    change[1] -= 1
                elif change[1]==0:
                    change[0] -=3
                if change[0] <0:
                    return False
        return all(count>=0 for count in change)
```
> - Time: O(N)
> - Space: O(1)
## 406. Queue Reconstruction by Height
[Leetcode Link](https://leetcode.cn/problems/queue-reconstruction-by-height/description/)-Medium
### Description
>Given an integer array nums and an integer k, modify the array in the following way:
>choose an index i and replace nums[i] with -nums[i].You should apply this process exactly k times. You may choose the same index i multiple times.
>Return the largest possible sum of the array after modifying it in this way.
>>**Example 1:**
>>**Input:**
>>people = [[7,0],[4,4],[7,1],[5,0],[6,1],[5,2]]
>>**Output:**
>>[[5,0],[7,0],[5,2],[6,1],[4,4],[7,1]]
>>**Explanation:** Person 0 has height 5 with no other people taller or the same height in front. Person 1 has height 7 with no other people taller or the same height in front.
>>Person 2 has height 5 with two persons taller or the same height in front, which is person 0 and 1. Person 3 has height 6 with one person taller or the same height in front, which is person 1.
>>Person 4 has height 4 with four people taller or the same height in front, which are people 0, 1, 2, and 3. Person 5 has height 7 with one person taller or the same height in front, which is person 1.
>>Hence [[5,0],[7,0],[5,2],[6,1],[4,4],[7,1]] is the reconstructed queue.
### Code
>也需要比较两边，先把身高进行排序，再按index进行排序。
>然后遍历，按index插入到相应index位置即可。
```python
class Solution:
    def reconstructQueue(self, people: List[List[int]]) -> List[List[int]]:
        ##h从大到小排，k从小到大排
        people.sort(key = lambda x:(-x[0],x[1]))
        que = []
        for i in people:
            que.insert(i[1],i) ##insert(index,item)
        return que
```
> - Time: O(N^2)
> - Space: O(N)
## 今日心得
- 贪心题干都非常复杂，像数学应用题，可以多画图，方便理清思路。画图可以让题干更加清晰。
