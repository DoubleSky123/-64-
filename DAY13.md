# DAY 13｜144. Binary Tree Preorder Traversal 145. Binary Tree Postorder Traversal 94. Binary Tree Inorder Traversal 102. Binary Tree Level Order Traversal
## 学习内容
[二叉树理论基础](https://programmercarl.com/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)
- 二叉树的种类: 
  - 满二叉树、完全二叉树、二叉搜索树、平衡二叉搜索树
- 二叉树的存储方式: 
  - **顺序存储:** 顺序存储的元素在内存中是连续分布的，通常用数组来存储。如果父节点的数组下标是 i，那么它的左孩子就是 i * 2 + 1，右孩子就是 i * 2 + 2。
  - **链式存储:** 链式存储是通过指针把分布在散落在各个地址的节点连接起来，链式存储的二叉树定义实现如下：
    ``` python
    class TreeNode:
    def __init__(self, val, left = None, right = None):
        self.val = val
        self.left = left
        self.right = right
    ```
- 二叉树的遍历方式
  - 深度优先遍历
    1. 前序遍历：中左右（递归法、迭代法）
    2. 中序遍历：左中右（递归法、迭代法）
    3. 后序遍历：左右中（递归法、迭代法）
  - 广度优先遍历
    - 层序遍历（迭代法）
- 递归遍历三部曲
  1. 确定递归函数的参数和返回值
  2. 确定终止条件
  3. 确定单层递归的逻辑
## 150. Evaluate Reverse Polish Notation
[Leetcode Link](https://leetcode.cn/problems/evaluate-reverse-polish-notation/description/)-Medium
### Description
>You are given an array of strings tokens that represents an arithmetic expression in a Reverse Polish Notation.
>Evaluate the expression. Return an integer that represents the value of the expression.Note that:
>The valid operators are '+', '-', '*', and '/'.
>Each operand may be an integer or another expression.
>The division between two integers always truncates toward zero.
>There will not be any division by zero.
>The input represents a valid arithmetic expression in a reverse polish notation.
>The answer and all the intermediate calculations can be represented in a 32-bit integer.
>>**Example 1:**
>>**Input:**
>>tokens = ["2","1","+","3","*"]
>>**Output:**
>>9
>>**Explanation:**
>>((2 + 1) * 3) = 9
### Code
>逆波兰表达式，栈经典应用。遍历数组的时候，遇到数字就入栈，遇到运算符号就弹出计算，再把计算结果入栈，最后栈内的结果即是最终结果。
>并不难，但是对于python中的运算符号需要有额外处理。
```python
from operator import add, sub, mul
def add(x, y): return x + y
def sub(x, y): return x - y
def mul(x, y): return x * y
func = self.op_map[token]
result = func(op1, op2)
## 等价于写成一行
result = self.op_map[token](op1, op2)
```
```python
from operator import add, sub, mul

def div(x, y):
    # 使用整数除法的向零取整方式
    return int(x / y) if x * y > 0 else -(abs(x) // abs(y))

class Solution(object):
    op_map = {'+': add, '-': sub, '*': mul, '/': div}
    
    def evalRPN(self, tokens: List[str]) -> int:
        stack = []
        for token in tokens:
            if token not in {'+', '-', '*', '/'}:
                stack.append(int(token))
            else:
                op2 = stack.pop()
                op1 = stack.pop()
                stack.append(self.op_map[token](op1, op2))  # 第一个出来的在运算符后面
        return stack.pop()
```
> - Time: O(N)
> - Space: O(N)
## 239. Sliding Window Maximum
[Leetcode Link](https://leetcode.cn/problems/sliding-window-maximum/description/)-Hard
### Description
>You are given an array of integers nums, there is a sliding window of size k which is moving from the very left of the array to the very right.
>You can only see the k numbers in the window. Each time the sliding window moves right by one position.
>Return the max sliding window.
>>**Example 1:**
>>**Input:**
>>nums = [1,3,-1,-3,5,3,6,7], k = 3
>>**Output:**
>>[3,3,5,5,6,7]
### Code
>单调队列实现。维护单调队列出口始终是最大值，使得它的前面不能有比它更大的值，如果比它小，则入队列。
>当出口值超出窗口范围时，pop掉，如何实现，遍历的时候出口值==遍历值时。
```python
class MyQueue:
    def __init__(self):
        self.queue = deque()
    def pop(self,value):
        if self.queue and value == self.queue[0]:
            self.queue.popleft()
    def push(self,value):
        while self.queue and value > self.queue[-1]:
            self.queue.pop()
        self.queue.append(value)
    def front(self):
        return self.queue[0]

class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        queue = MyQueue()
        result = []
        for i in range(k):
            queue.push(nums[i])
        result.append(queue.front())
        for i in range(k,len(nums)):
            queue.pop(nums[i-k])
            queue.push(nums[i])
            result.append(queue.front())
        return result
```
> - Time: O(N)
> - Space: O(K)
## 347. Top K Frequent Elements
[Leetcode Link](https://leetcode.cn/problems/top-k-frequent-elements/description/)-Medium
### Description
>Given an integer array nums and an integer k, return the k most frequent elements. You may return the answer in any order.
>>**Example 1:**
>>**Input:**
>> nums = [1,1,1,2,2,3], k = 2
>>**Output:**
>> [1,2]
### Code
>用优先级队列-小顶堆。首先用map统计每个数的频率，如果排序的话时间复杂是nlogn，如果用堆的话时间是nlogk。
>python有可以直接实现小顶堆的库函数heapq。heapq.heappush(priority_que,(freq,key))按从小到大排列，队列出口，也就是根节点是最小值。
>heapq.heappop(priority_que)，如果堆长度大于需要求的前k个字符，就pop。最后留下的就是最高频的。
```python
import heapq
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        map_ = {}
        for i in nums:
            map_[i] = map_.get(i,0) + 1
        priority_que = []
        for key,freq in map_.items():
            heapq.heappush(priority_que,(freq,key))
            print(priority_que)
            if len(priority_que) > k:
                heapq.heappop(priority_que)
                print(priority_que)
        result = [0] *k
        for i in range(k-1,-1,-1):
            result[i] = heapq.heappop(priority_que)[1]
        return result
```
> - Time: O(Nlogk)
> - Space: O(N)
## 今日心得
- 滑动窗口和堆应用这两题都较难。滑动窗口考察了单调队列的思路。堆应用考察了小顶堆的应用。逆波兰表达式则是经典栈应用，并且应用了python的operator库。都需要熟练记住。
