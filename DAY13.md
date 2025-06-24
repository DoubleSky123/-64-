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
## 144. Binary Tree Preorder Traversal
[Leetcode Link](https://leetcode.cn/problems/binary-tree-preorder-traversal/description/)-Easy
### Description
>Given the root of a binary tree, return the preorder traversal of its nodes' values.
>>**Example 1:**
>>**Input:**
>>root = [1,null,2,3]
>>**Output:**
>>[1,2,3]
### Code
#### Method1 递归
```python
class Solution:
    def preorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        result = []
        def dfs(cur):
            if not cur:
                return
            result.append(cur.val)
            dfs(cur.left)
            dfs(cur.right)
        dfs(root)
        return result
```
> - Time: O(N)
> - Space: O(N)
#### Method2 迭代
>用栈注意先遍历右子树再遍历左子树，弹出加入到结果就是左到右。
```python
class Solution:
    def preorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        if not root:
            return []
        stack = [root]
        result = []
        while stack:
            node = stack.pop()
            result.append(node.val)
            if node.right:
                stack.append(node.right)
            if node.left:
                stack.append(node.left)
        return result
```
> - Time: O(N)
> - Space: O(N)
## 94. Binary Tree Inorder Traversal
[Leetcode Link](https://leetcode.cn/problems/binary-tree-inorder-traversal/description/)-Easy
### Description
>Given the root of a binary tree, return the inorder traversal of its nodes' values.
>>**Example 1:**
>>**Input:**
>>root = [1,null,2,3]
>>**Output:**
>>[1,3,2]
### Code
#### Method1 递归
```python
class Solution:
    def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        result = []
        def dfs(cur):
            if not cur:
                return 

            dfs(cur.left)
            result.append(cur.val)
            dfs(cur.right)

        dfs(root)

        return result
```
> - Time: O(N)
> - Space: O(N)
#### Method2 迭代
>中序迭代和前后不一样，因为要从左子树开始，所以要用一个指针，从左往右遍历。
```python
class Solution:
    def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        if not root:
            return []
        stack = []
        result = []
        cur = root
        while cur or stack:
            if cur:
                stack.append(cur)
                cur = cur.left
            else:
                cur = stack.pop()
                result.append(cur.val)
                cur = cur.right
        return result
```
> - Time: O(N)
> - Space: O(N)
## 145. Binary Tree Postorder Traversal
[Leetcode Link](https://leetcode.cn/problems/binary-tree-postorder-traversal/description/)-Easy
### Description
>Given the root of a binary tree, return the postorder traversal of its nodes' values.
>>**Example 1:**
>>**Input:**
>>root = [1,null,2,3]
>>**Output:**
>>[3,2,1]
### Code
#### Method1 递归
```python
class Solution:
    def postorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        result = []
        def dfs(cur):
            if not cur:
                return
            dfs(cur.left)
            dfs(cur.right)
            result.append(cur.val)
        
        dfs(root)
        return result
```
> - Time: O(N)
> - Space: O(N)
#### Method2 迭代
>和前序一样，把左右子树遍历顺序颠倒，变成中左右，得到中右左的数组，然后反转数组即是左右中。
```python
class Solution:
    def postorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        if not root:
            return []
        stack = [root]
        result = []
        while stack:
            node = stack.pop()
            result.append(node.val)
            if node.left:
                stack.append(node.left)
            if node.right:
                stack.append(node.right)
        return result[::-1]
```
> - Time: O(N)
> - Space: O(N)
## 102. Binary Tree Level Order Traversal
[Leetcode Link](https://leetcode.cn/problems/binary-tree-level-order-traversal/description/)- Medium
### Description
>Given the root of a binary tree, return the level order traversal of its nodes' values. (i.e., from left to right, level by level).
>>**Example 1:**
>>**Input:**
>>root = [3,9,20,null,null,15,7]
>>**Output:**
>>[[3],[9,20],[15,7]]
### Code
>层序遍历是BFS，将树一层层遍历，用队列。
>记录每一层的长度，然后一个个加入到队列再弹出。
```python
class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        if not root:
            return []
        queue = deque([root])
        result = []
        while queue:
            level = []
            for _ in range(len(queue)):
                node = queue.popleft()
                level.append(node.val)
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
            result.append(level)
        return result
```
> - Time: O(N)
> - Space: O(N)
## 今日心得
- 递归遍历顺序无论是哪种，中就是单层递归处理逻辑，左右就是套上递归函数。
