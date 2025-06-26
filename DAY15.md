# DAY 15｜222. Count Complete Tree Nodes 110. Balanced Binary Tree 257. Binary Tree Paths 404. Sum of Left Leaves
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
## 222. Count Complete Tree Nodes
[Leetcode Link](https://leetcode.cn/problems/count-complete-tree-nodes/description/)-Easy
### Description
>Given the root of a complete binary tree, return the number of the nodes in the tree.
>According to Wikipedia, every level, except possibly the last, is completely filled in a complete binary tree, and all nodes in the last level are as far left as possible.
>It can have between 1 and 2h nodes inclusive at the last level h.
>Design an algorithm that runs in less than O(n) time complexity.
>>**Example 1:**
>>**Input:**
>>root = [1,2,3,4,5,6]
>>**Output:**
>>6
### Code
#### Method1 普通递归
```python
class Solution:
    def countNodes(self, root: Optional[TreeNode]) -> int:
        return self.getCount(root)
    def getCount(self,root)->int:
        if not root:
            return 0
        left = self.getCount(root.left)
        right = self.getCount(root.right)
        result = 1+ left + right
        return result
```
> - Time: O(N)
> - Space: O(H)
#### Method2 完全二叉树递归
```python
class Solution:
    def countNodes(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        left = root.left
        right = root.right
        leftDepth = 0
        rightDepth = 0
        while left:
            left = left.left
            leftDepth += 1
        while right:
            right = right.right
            rightDepth += 1
        if leftDepth == rightDepth:
            return (2<<leftDepth)-1
        return self.countNodes(root.left) + self.countNodes(root.right) + 1
```
> - Time: O(N)
> - Space: O(H)
## 110. Balanced Binary Tree
[Leetcode Link](https://leetcode.cn/problems/balanced-binary-tree/description/)-Easy
### Description
>Given a binary tree, determine if it is height-balanced.
>>**Example 1:**
>>**Input:**
>>root = [3,9,20,null,null,15,7]
>>**Output:**
>>true
### Code
#### Method1 递归
```python
class Solution:
    def isBalanced(self, root: Optional[TreeNode]) -> bool:
        if self.get_height(root) != -1:
            return True
        else:
            return False 
    def get_height(self,root):
        #base case
        if not root:
            return 0
        if (left_height := self.get_height(root.left)) == -1:
            return -1
        if (right_height := self.get_height(root.right)) ==-1:
            return -1
        if abs(left_height-right_height)>1:
            return -1
        else:
            return 1+max(left_height,right_height)
```
> - Time: O(N)
> - Space: O(H)
#### Method2 迭代
```python

```
> - Time: O(N)
> - Space: O(N)
## 257. Binary Tree Paths
[Leetcode Link](https://leetcode.cn/problems/binary-tree-paths/description/)-Easy
### Description
>Given the root of a binary tree, return all root-to-leaf paths in any order.
>A leaf is a node with no children.
>>**Example 1:**
>>**Input:**
>>root = [1,2,3,null,5]
>>**Output:**
>>["1->2->5","1->3"]
### Code
#### Method1 递归
```python
class Solution:
    def binaryTreePaths(self, root: Optional[TreeNode]) -> List[str]:
        path = []
        result = []
        self.traversal(root,path,result)
        return result


    def traversal(self,root,path,result):
        path.append(root.val)
        if not root.left and not root.right:
            allpath = "->".join(map(str,path))
            result.append(allpath)
            return 
        if root.left:
            self.traversal(root.left,path,result)
            path.pop()
        if root.right:
            self.traversal(root.right,path,result)
            path.pop()
        return result
```
> - Time: O(N)
> - Space: O(N)
#### Method2 迭代
```python

```
> - Time: O(N)
> - Space: O(N)
## 404. Sum of Left Leaves
[Leetcode Link](https://leetcode.cn/problems/sum-of-left-leaves/description/)- Easy
### Description
>Given the root of a binary tree, return the sum of all left leaves.
>A leaf is a node with no children. A left leaf is a leaf that is the left child of another node.
>>**Example 1:**
>>**Input:**
>>root = [3,9,20,null,null,15,7]
>>**Output:**
>>24
>>**Explanation:**
>>There are two left leaves in the binary tree, with values 9 and 15 respectively.
### Code
#### Method1 递归
```python
class Solution:
    def sumOfLeftLeaves(self, root: Optional[TreeNode]) -> int:
        self.leftSum = 0

        def dfs(node):
            
            if not node:
                return 
            if node.left and not node.left.left and not node.left.right:
                self.leftSum += node.left.val
            dfs(node.left)
            dfs(node.right)

        dfs(root)
        return self.leftSum
```
> - Time: O(N)
> - Space: O(H)
#### Method2 迭代
```python

```
> - Time: O(N)
> - Space: O(N)
## 今日心得
