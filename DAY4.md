# DAY 4｜24. Swap Nodes in Pairs 19. Remove Nth Node From End of List 面试题 02.07. Intersection of Two Linked Lists LCCI 142. Linked List Cycle II
## 学习内容
[24讲解](https://programmercarl.com/0024.%E4%B8%A4%E4%B8%A4%E4%BA%A4%E6%8D%A2%E9%93%BE%E8%A1%A8%E4%B8%AD%E7%9A%84%E8%8A%82%E7%82%B9.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)
[19讲解](https://programmercarl.com/0019.%E5%88%A0%E9%99%A4%E9%93%BE%E8%A1%A8%E7%9A%84%E5%80%92%E6%95%B0%E7%AC%ACN%E4%B8%AA%E8%8A%82%E7%82%B9.html)
[链表相交讲解](https://programmercarl.com/%E9%9D%A2%E8%AF%95%E9%A2%9802.07.%E9%93%BE%E8%A1%A8%E7%9B%B8%E4%BA%A4.html)
[142讲解](https://programmercarl.com/0142.%E7%8E%AF%E5%BD%A2%E9%93%BE%E8%A1%A8II.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)
## 24. Swap Nodes in Pairs
[Leetcode Link](https://leetcode.cn/problems/swap-nodes-in-pairs/description/)-Medium
### Description
>Given a linked list, swap every two adjacent nodes and return its head.
>You must solve the problem without modifying the values in the list's nodes (i.e., only nodes themselves may be changed.)
>>**Example 1:**
>>**Input:** head = `[1,2,3,4]`
>>**Output:** `[2,1,4,3]`
### Code
#### Method1 虚拟头节点
>注意用temp保存指针
```python
class Solution:
    def swapPairs(self, head: Optional[ListNode]) -> Optional[ListNode]:
        dummyhead = ListNode(next = head)
        cur = dummyhead
        while cur.next is not None and cur.next.next is not None:
            # cur.next指向2，但是会改变cur.next所以要用temp保存，不然后面2就不能指向1了，1是cur.next
            temp = cur.next  
            # 1要指向3，但是因为2已经指向1，所以也要保存3，不然2到3就没有了
            temp1 = cur.next.next.next
            cur.next = cur.next.next 
            cur.next.next = temp
            temp.next = temp1
            cur = cur.next.next
        return dummyhead.next
```
> - Time: O(N)
> - Space: O(1)
#### Method2 递归
## 19. Remove Nth Node From End of List
[Leetcode Link](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/description/)-Medium
### Description
>Given the head of a linked list, remove the nth node from the end of the list and return its head.
>>**Example 1:**
>>**Input:** head = `[1,2,3,4,5]`, n = 2
>>**Output:** `[1,2,3,5]`
### Code
>双指针，倒数第n个节点即是正数len-n个节点。
>注意删除节点是cur.next
```python
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        dummyhead = ListNode(next = head)
        fast = slow = dummyhead
        # 双指针，当fast先走n步，剩下len-n，和slow一起走len-n步就是倒数第n个节点
        # 因为删除节点要在下一个所以fast要走n+1步，所以while n>=0
        while n >= 0:
            fast = fast.next
            n -= 1
        while fast is not None:
            fast = fast.next
            slow = slow.next
        slow.next = slow.next.next

        return dummyhead.next
```
> - Time: O(N)
> - Space: O(1)
## 面试题 02.07. Intersection of Two Linked Lists LCCI
[Leetcode Link](https://leetcode.cn/problems/intersection-of-two-linked-lists-lcci/description/)-Easy
### Description
>Given two (singly) linked lists, determine if the two lists intersect.
>Return the inter­ secting node. Note that the intersection is defined based on reference, not value.
>That is, if the kth node of the first linked list is the exact same node (by reference) as the jth node of the second linked list, then they are intersecting.
>>**Example 1:**
>>**Input:** intersectVal = 8, listA = [4,1,8,4,5], listB = [5,0,1,8,4,5], skipA = 2, skipB = 3
>>**Output:** Reference of the node with value = 8
>>**Input Explanation:** The intersected node's value is 8 (note that this must not be 0 if the two lists intersect).
>>From the head of A, it reads as [4,1,8,4,5]. From the head of B, it reads as [5,0,1,8,4,5].
>>There are 2 nodes before the intersected node in A; There are 3 nodes before the intersected node in B.
### Code
>首先需要计算出两个链表的长度，将较长链表指针移动到和较短链表末尾对齐的位置，然后一起移动。判断如果两个链表指针指向相同位置，即为相交。
```python
class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        lenA, lenB = 0,0
        cur = headA
        while cur is not None:
            cur = cur.next
            lenA += 1
        cur = headB
        while cur is not None:
            cur = cur.next
            lenB += 1
        
        curA, curB = headA, headB
        if lenA > lenB:
            curA, curB = curB, curA
            lenA, lenB = lenB, lenA
        for _ in range(lenB-lenA):
            curB = curB.next
        while curA:
            if curA == curB:
                return curA
            else:
                curA = curA.next
                curB = curB.next
        return None
```
> - Time: O(M+N)
> - Space: O(1)
## 142. Linked List Cycle II
[Leetcode Link](https://leetcode.cn/problems/linked-list-cycle-ii/description/)-Medium
### Description
>Given the head of a linked list, return the node where the cycle begins. If there is no cycle, return null.
>There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the next pointer.
>Internally, pos is used to denote the index of the node that tail's next pointer is connected to (0-indexed).
>It is -1 if there is no cycle. Note that pos is not passed as a parameter.Do not modify the linked list.
>>**Example 1:**
>>**Input:** head = `[3,2,0,-4]`, pos = 1
>>**Output:** tail connects to node index 1
>>**Explanation:** There is a cycle in the linked list, where tail connects to the second node.
### Code
>这是一道数学题，使快指针速度是慢指针两倍，那么他们一定会在环内相遇。设head到环入口距离为x，入口到相遇点距离为y，相遇点再到入口为z。
>可以得出等式2(x+y) = n(y+z)+(x+y),n为快指针圈数。可以得到(x+y) = n(y+z)，假设n为1可以得到x=z。
>从相遇点再设两个指针一起走直到相遇，即为环入口。
```python
class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        fast = slow = head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next

            if slow == fast:
                index1 = slow
                index2 = head
                while index1 != index2:
                    index1 = index1.next
                    index2 = index2.next
                return index1
        return None
```
> - Time: O(N)
> - Space: O(1)
## 今日心得
- 今天学习的链表题目更偏数学，可以发现链表使用双指针法频率很高。碰到链表题目，需要先理清逻辑，`画图很重要！`链表题目画图非常重要，画图更加清晰，也便于理清数学逻辑。
