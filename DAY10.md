# DAY 10｜232. Implement Queue using Stacks 225. Implement Stack using Queues 20. Valid Parentheses 1047. Remove All Adjacent Duplicates In String
## 学习内容
[栈与队列理论基础](https://programmercarl.com/%E6%A0%88%E4%B8%8E%E9%98%9F%E5%88%97%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html)
## 232. Implement Queue using Stacks
[Leetcode Link](https://leetcode.cn/problems/implement-queue-using-stacks/description/)-Easy
### Description
>Implement a first in first out (FIFO) queue using only two stacks.
>The implemented queue should support all the functions of a normal queue (push, peek, pop, and empty).
>Implement the MyQueue class:
>void push(int x) Pushes element x to the back of the queue.
>int pop() Removes the element from the front of the queue and returns it.
>int peek() Returns the element at the front of the queue.
>boolean empty() Returns true if the queue is empty, false otherwise.
>>**Example 1:**
>>**Input:**
>>["MyQueue", "push", "push", "peek", "pop", "empty"]
>>[[], [1], [2], [], [], []]
>>**Output:**
>>[null, null, null, 1, 1, false]
>>**Explanation:**
>>MyQueue myQueue = new MyQueue();
>>myQueue.push(1); // queue is: [1]
>>myQueue.push(2); // queue is: [1, 2] (leftmost is front of the queue)
>>myQueue.peek(); // return 1
>>myQueue.pop(); // return 1, queue is [2]
>>myQueue.empty(); // return false
### Code
```python
class MyQueue:

    def __init__(self):
        self.stack_in = []
        self.stack_out = []

    def push(self, x: int) -> None:
        self.stack_in.append(x)

    def pop(self) -> int:
        if self.empty():
            return None
        if self.stack_out:
            return self.stack_out.pop()
        else:
            for i in range(len(self.stack_in)):
                self.stack_out.append(self.stack_in.pop())
            return self.stack_out.pop()

    def peek(self) -> int:
        ans = self.pop()
        self.stack_out.append(ans)
        return ans

    def empty(self) -> bool:
        return not (self.stack_in or self.stack_out)

# Your MyQueue object will be instantiated and called as such:
# obj = MyQueue()
# obj.push(x)
# param_2 = obj.pop()
# param_3 = obj.peek()
# param_4 = obj.empty()
```
## 225. Implement Stack using Queues
[Leetcode Link](https://leetcode.cn/problems/implement-stack-using-queues/description/)-Easy
### Description
>Implement a last-in-first-out (LIFO) stack using only two queues. The implemented stack should support all the functions of a normal stack (push, top, pop, and empty).
>Implement the MyStack class:
>void push(int x) Pushes element x to the top of the stack.
>int pop() Removes the element on the top of the stack and returns it.
>int top() Returns the element on the top of the stack.
>boolean empty() Returns true if the stack is empty, false otherwise.
>>**Example 1:**
>>**Input:**
>>["MyStack", "push", "push", "top", "pop", "empty"]
>>[[], [1], [2], [], [], []]
>>**Output:**
>>[null, null, null, 2, 2, false]
### Code
```python
class MyStack:

    def __init__(self):
        self.queue = deque()

    def push(self, x: int) -> None:
        self.queue.append(x)

    def pop(self) -> int:
        if self.empty():
            return None
        for i in range(len(self.queue)-1):
            self.queue.append(self.queue.popleft())
        return self.queue.popleft()

    def top(self) -> int:
        if self.empty():
            return None
        ans = self.pop()
        self.queue.append(ans)
        return ans


    def empty(self) -> bool:
        return not self.queue


# Your MyStack object will be instantiated and called as such:
# obj = MyStack()
# obj.push(x)
# param_2 = obj.pop()
# param_3 = obj.top()
# param_4 = obj.empty()
```
## 20. Valid Parentheses
[Leetcode Link](https://leetcode.cn/problems/valid-parentheses/description/)-Easy
### Description
>Given a string s containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.
>An input string is valid if:
>Open brackets must be closed by the same type of brackets.
>Open brackets must be closed in the correct order.
>Every close bracket has a corresponding open bracket of the same type.
>>**Example 1:**
>>**Input:**
>> s = "()[]{}"
>>**Output:**
>>true
### Code
```python
class Solution:
    def isValid(self, s: str) -> bool:
        stack = []
        for i in s:
            if i == '(':
                stack.append(')')
            elif i == '[':
                stack.append(']')
            elif i == '{':
                stack.append('}')
            elif not stack or stack[-1] != i:
                return False
            else:
                stack.pop()
        return True if not stack else False
```
> - Time: O(N)
> - Space: O(N)
## 1047. Remove All Adjacent Duplicates In String
[Leetcode Link](https://leetcode.cn/problems/remove-all-adjacent-duplicates-in-string/description/)-Easy
### Description
>You are given a string s consisting of lowercase English letters. A duplicate removal consists of choosing two adjacent and equal letters and removing them.
>We repeatedly make duplicate removals on s until we no longer can.
>Return the final string after all such duplicate removals have been made. It can be proven that the answer is unique.
>>**Example 1:**
>>**Input:**
>> s = "abbaca"
>>**Output:**
>>"ca"
>>**Explanation:**
>>For example, in "abbaca" we could remove "bb" since the letters are adjacent and equal, and this is the only possible move.
>>The result of this move is that the string is "aaca", of which only "aa" is possible, so the final string is "ca".
### Code
```python
class Solution:
    def removeDuplicates(self, s: str) -> str:
        stack = []
        for i in s:
            if not stack or stack[-1] != i:
                stack.append(i)
            else:
                stack.pop()

        return ''.join(stack)=
```
> - Time: O(N)
> - Space: O(N)
## 今日心得
- 开始栈和队列，python可以调用`deque()`双端队列，`popleft()`来实现队列。
- 栈可以用来解决左右匹配问题。
