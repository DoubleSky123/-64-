# DAY 14｜226. Invert Binary Tree 101. Symmetric Tree 104. Maximum Depth of Binary Tree 111. Minimum Depth of Binary Tree
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
## 226. Invert Binary Tree
[Leetcode Link](https://leetcode.cn/problems/invert-binary-tree/description/)-Easy
### Description
>Given the root of a binary tree, invert the tree, and return its root.
>>**Example 1:**
>>**Input:**
>>root = [4,2,7,1,3,6,9]
>>**Output:**
>>[4,7,2,9,6,3,1]
### Code
#### Method1 递归
>前序或后序遍历，不能用中序。因为要交换左右子树，所以要左右一起处理。
```python
class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root:
            return 
        # root.left,root.right = root.right,root.left
        self.invertTree(root.left)
        self.invertTree(root.right)
        root.left,root.right = root.right,root.left
        return root
```
> - Time: O(N)
> - Space: O(H)
#### Method2 层序
```python
class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root:
            return
        queue = deque([root])
        while queue:
            for _ in range(len(queue)):
                node = queue.popleft()
                node.left,node.right = node.right,node.left
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
        return root
```
> - Time: O(N)
> - Space: O(N)
## 101. Symmetric Tree
[Leetcode Link](https://leetcode.cn/problems/symmetric-tree/description/)-Easy
### Description
>Given the root of a binary tree, check whether it is a mirror of itself (i.e., symmetric around its center).
>>**Example 1:**
>>**Input:**
>>root = [1,2,2,3,4,4,3]
>>**Output:**
>>true
### Code
#### Method1 递归
>后序遍历，先处理左右子树，左右子树往上返回结果再处理中。
>中的处理逻辑需要考虑几种情况：左为空右不为空，右为空左不为空，左右值不相等，左右都为空。
>因为要比较是否对称，所以要传入两个参数。
```python
class Solution:
    def isSymmetric(self, root: Optional[TreeNode]) -> bool:
        if not root:
            return True
        return self.dfs(root.left,root.right)

    def dfs(self,left,right)->bool:
        if left is None and right is not None:
            return False
        elif left is not None and right is None:
            return False
        elif not left and not right:
            return True
        elif left.val != right.val:
            return False
        ##后序遍历
        outside = self.dfs(left.left,right.right) ## 左
        inside = self.dfs(left.right,right.left) ## 右
        result = outside and inside ## 中

        return result
```
> - Time: O(N)
> - Space: O(H)
#### Method2 迭代
```python

```
> - Time: O(N)
> - Space: O(N)
## 104. Maximum Depth of Binary Tree
[Leetcode Link](https://leetcode.cn/problems/maximum-depth-of-binary-tree/description/)-Easy
### Description
>Given the root of a binary tree, return its maximum depth.
>A binary tree's maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.
>>**Example 1:**
>>**Input:**
>>root = [3,9,20,null,null,15,7]
>>**Output:**
>>3
### Code
#### Method1 递归
>前序或后序，后序遍历更加方便，前序遍历需要增加一个参数。前序是深度，后序是高度。
>遍历左右子树，将左右子树高度取最大值+1，因为根节点算1.
```python
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        return self.getDepth(root)
    def getDepth(self,root):
        if not root:
            return 0
        leftdepth = self.getDepth(root.left)
        rightdepth = self.getDepth(root.right)
        height = 1+max(leftdepth,rightdepth)

        return height
```
> - Time: O(N)
> - Space: O(H)
#### Method2 迭代
```python

```
> - Time: O(N)
> - Space: O(N)
## 111. Minimum Depth of Binary Tree
[Leetcode Link](https://leetcode.cn/problems/minimum-depth-of-binary-tree/description/)- Easy
### Description
>Given a binary tree, find its minimum depth.
The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.
>Note: A leaf is a node with no children.
>>**Example 1:**
>>**Input:**
>>root = [3,9,20,null,null,15,7]
>>**Output:**
>>2
### Code
#### Method1 递归
>和最大深度不同是，最小深度要处理如果左右子树有一个为空的情况，需要返回另一棵子树深度。
>因为题目中说到叶子的距离，没有子树不能算。所以还是要去找最短的叶子深度。
```python
class Solution:
    def minDepth(self, root: Optional[TreeNode]) -> int:
        return self.getheight(root)

    def getheight(self,root) -> int:
        if not root:
            return 0
        
        left = self.getheight(root.left)
        right = self.getheight(root.right)
        if left == 0 and right!= 0:
            return 1+right
        if left != 0 and right == 0:
            return 1+left 
        result = 1+min(left,right)
        return result
```
> - Time: O(N)
> - Space: O(N)
#### Method2 迭代
```python

```
> - Time: O(N)
> - Space: O(N)
## 今日心得
- 二叉树遍历题目，一定要先确定遍历顺序，是理解题目的关键。
- 后续把迭代法补上。
