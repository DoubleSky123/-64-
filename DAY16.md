# DAY 16｜513. Find Bottom Left Tree Value 112. Path Sum 106. Construct Binary Tree from Inorder and Postorder Traversal
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
## 513. Find Bottom Left Tree Value
[Leetcode Link](https://leetcode.cn/problems/find-bottom-left-tree-value/description/)-Medium
### Description
>Given the root of a binary tree, return the leftmost value in the last row of the tree.
>>**Example 1:**
>>**Input:**
>>root = [1,2,3,4,null,5,6,null,null,7]
>>**Output:**
>>7
### Code
#### Method1 递归
>
```python
class Solution:
    def findBottomLeftValue(self, root: Optional[TreeNode]) -> int:
        self.maxDepth = float('-inf')
        self.result = 0

        def dfs(node,depth):
            if not node.left and not node.right:
                if depth>self.maxDepth:
                    self.maxDepth = depth
                    self.result = node.val
                    return 
            if node.left:
                dfs(node.left,depth+1)
            if node.right:
                dfs(node.right,depth+1)
        
        dfs(root,0)
        return self.result
```
> - Time: O(N)
> - Space: O(H)
#### Method2 迭代
```python

```
> - Time: O(N)
> - Space: O(H)
## 112. Path Sum
[Leetcode Link](https://leetcode.cn/problems/path-sum/description/)-Easy
### Description
>Given the root of a binary tree and an integer targetSum, return true if the tree has a root-to-leaf path such that adding up all the values along the path equals targetSum.
>A leaf is a node with no children.
>>**Example 1:**
>>**Input:**
>>root = [5,4,8,11,null,13,4,7,2,null,null,null,1], targetSum = 22
>>**Output:**
>>true
>>**Explanation:**
>>The root-to-leaf path with the target sum is shown.
### Code
#### Method1 递归
>
```python
class Solution:
    def hasPathSum(self, root: Optional[TreeNode], targetSum: int) -> bool:
        if not root:  # 添加空节点检查
            return False
        return self.traversal(root,targetSum)
         
    def traversal(self,node,targetSum):
        # if not node:
        #     return False

        targetSum -= node.val
        if not node.left and not node.right:
            return targetSum == 0
        if node.left:
            if self.traversal(node.left,targetSum):
                return True
        if node.right:
            if self.traversal(node.right,targetSum):
                return True
        return False
```
> - Time: O(N)
> - Space: O(H)
#### Method2 迭代
```python

```
> - Time: O(N)
> - Space: O(N)
## 106. Construct Binary Tree from Inorder and Postorder Traversal
[Leetcode Link](https://leetcode.cn/problems/construct-binary-tree-from-inorder-and-postorder-traversal/description/)-Medium
### Description
>Given two integer arrays inorder and postorder where inorder is the inorder traversal of a binary tree and postorder is the postorder traversal of the same tree, construct and return the binary tree.
>>**Example 1:**
>>**Input:**
>>inorder = [9,3,15,20,7], postorder = [9,15,7,20,3]
>>**Output:**
>>[3,9,20,null,null,15,7]
### Code
>六步法
```python
class Solution:
    def buildTree(self, inorder: List[int], postorder: List[int]) -> Optional[TreeNode]:
        #第一步如果其中一个数组为空，那么返回空
        if len(postorder) ==0:
            return None
        #第二步找到后序数组最后一个字符即为中的根节点
        root_val = postorder[-1]
        root = TreeNode(root_val)
        #第三步在中序数组中找到中根节点的位置
        cut_index = inorder.index(root_val)
        #第四步切割中序数组
        inorder_left = inorder[:cut_index]
        inorder_right = inorder[cut_index+1:]
        #第五步根据中序数组的左树切割后序数组
        postorder_left = postorder[:len(inorder_left)]
        postorder_right = postorder[len(inorder_left):-1]
        #第六步递归
        root.left = self.buildTree(inorder_left,postorder_left)
        root.right = self.buildTree(inorder_right,postorder_right)
        return root
```
> - Time: O(N^2)
> - Space: O(N)
## 今日心得
- 如果需要搜索整棵二叉树，那么递归函数就不要返回值，如果要搜索其中一条符合条件的路径，递归函数就需要返回值，因为遇到符合条件的路径了就要及时返回。
