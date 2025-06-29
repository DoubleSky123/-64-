# DAY 18｜530. Minimum Absolute Difference in BST 501. Find Mode in Binary Search Tree 236. Lowest Common Ancestor of a Binary Tree
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
  - 深度优先遍历（栈）
    1. 前序遍历：中左右（递归法、迭代法）
    2. 中序遍历：左中右（递归法、迭代法）
    3. 后序遍历：左右中（递归法、迭代法）
  - 广度优先遍历（队列）
    - 层序遍历（迭代法）
- 递归遍历三部曲
  1. 确定递归函数的参数和返回值
  2. 确定终止条件
  3. 确定单层递归的逻辑
## 530. Minimum Absolute Difference in BST
[Leetcode Link](https://leetcode.cn/problems/minimum-absolute-difference-in-bst/description/)-Easy
### Description
>Given the root of a Binary Search Tree (BST), return the minimum absolute difference between the values of any two different nodes in the tree.
>>**Example 1:**
>>**Input:**
>>root = [4,2,6,1,3]
>>**Output:**
>>1
### Code
#### Method1 递归
>双指针+中序遍历
```python
class Solution:
    def __init__(self):
        self.min = float('inf')
        self.pre = None

    def getMinimumDifference(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 
        
        self.getMinimumDifference(root.left)
        if self.pre and abs(root.val-self.pre.val) < self.min:
            self.min = abs(root.val-self.pre.val)
        self.pre = root
        self.getMinimumDifference(root.right)
        return self.min
```
> - Time: O(N)
> - Space: O(1)
#### Method2 迭代
```python

```
> - Time: O(N)
> - Space: O(H)
## 501. Find Mode in Binary Search Tree
[Leetcode Link](https://leetcode.cn/problems/find-mode-in-binary-search-tree/description/)-Easy
### Description
>Given the root of a binary search tree (BST) with duplicates, return all the mode(s) (i.e., the most frequently occurred element) in it.
>If the tree has more than one mode, return them in any order.
>Assume a BST is defined as follows:
>The left subtree of a node contains only nodes with keys less than or equal to the node's key.
>The right subtree of a node contains only nodes with keys greater than or equal to the node's key.
>Both the left and right subtrees must also be binary search trees.
>>**Example 1:**
>>**Input:**
>>root = [1,null,2,2]
>>**Output:**
>>[2]
### Code
#### Method1 递归
>双指针中序遍历，需要注意，当统计频率大于最大频率时，需要清空结果集self.result = [cur.val]，把前面记录的结果清空。
```python
class Solution:
    def __init__(self):
        self.maxCount = 0  # 最大频率
        self.count = 0  # 统计频率
        self.pre = None
        self.result = []

    def searchBST(self, cur):
        if cur is None:
            return

        self.searchBST(cur.left)  # 左
        # 中
        if self.pre is None:  # 第一个节点
            self.count = 1
        elif self.pre.val == cur.val:  # 与前一个节点数值相同
            self.count += 1
        else:  # 与前一个节点数值不同
            self.count = 1
        self.pre = cur  # 更新上一个节点

        if self.count == self.maxCount:  # 如果与最大值频率相同，放进result中
            self.result.append(cur.val)

        if self.count > self.maxCount:  # 如果计数大于最大值频率
            self.maxCount = self.count  # 更新最大频率
            self.result = [cur.val]  # 很关键的一步，不要忘记清空result，之前result里的元素都失效了

        self.searchBST(cur.right)  # 右
        return

    def findMode(self, root):
        self.count = 0
        self.maxCount = 0
        self.pre = None  # 记录前一个节点
        self.result = []

        self.searchBST(root)
        return self.result
```
> - Time: O(N)
> - Space: O(N)
#### Method2 迭代
```python

```
> - Time: O(N)
> - Space: O(N)
## 236. Lowest Common Ancestor of a Binary Tree
[Leetcode Link](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/description/)-Medium
### Description
>Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.
>According to the definition of LCA on Wikipedia:
>“The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants
>(where we allow a node to be a descendant of itself).”
>>**Example 1:**
>>**Input:**
>>root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
>>**Output:**
>>3
>>**Explanation:**
>>The LCA of nodes 5 and 1 is 3.
### Code
#### Method1 递归
>用后序遍历！！！很关键
>终止条件为当遍历节点等于p或者q直接返回，p和q为指针，可以直接相等。
>中处理逻辑为，因为需要返回节点，需要左右接住，分三种情况：如果左右都存在，已经找到公共祖先直接返回。左右哪边有就返回哪边。
```python
class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        
        if not root:
            return 
        if root == p or root == q:
            return root
        left = self.lowestCommonAncestor(root.left,p,q)
        right = self.lowestCommonAncestor(root.right,p,q)
        if left and right:
            return root
        elif not left and right:
            return right
        elif left and not right:
            return left
        else:
            return 
```
> - Time: O(N)
> - Space: O(1)
#### Method2 迭代
```python

```
> - Time: O(N)
> - Space: O(1)
## 今日心得
- BST题目需要活用双指针。
- 公共祖先用后序遍历，自下往上。
