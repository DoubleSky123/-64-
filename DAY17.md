# DAY 17｜654. Maximum Binary Tree 617. Merge Two Binary Trees 700. Search in a Binary Search Tree 98. Validate Binary Search Tree
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
## 654. Maximum Binary Tree
[Leetcode Link]((https://leetcode.cn/problems/maximum-binary-tree/description/))-Medium
### Description
>You are given an integer array nums with no duplicates. A maximum binary tree can be built recursively from nums using the following algorithm:
>Create a root node whose value is the maximum value in nums.
>Recursively build the left subtree on the subarray prefix to the left of the maximum value.
>Recursively build the right subtree on the subarray suffix to the right of the maximum value.
>Return the maximum binary tree built from nums.
>>**Example 1:**
>>**Input:**
>>nums = [3,2,1,6,0,5]
>>**Output:**
>>[6,3,5,null,2,0,null,null,1]
**Explanation:**
>> The recursive calls are as follow:
>>The largest value in [3,2,1,6,0,5] is 6. Left prefix is [3,2,1] and right suffix is [0,5].
>>The largest value in [3,2,1] is 3. Left prefix is [] and right suffix is [2,1].
>>Empty array, so no child.
>>The largest value in [2,1] is 2. Left prefix is [] and right suffix is [1].Empty array, so no child.Only one element, so child is a node with value 1.
>>The largest value in [0,5] is 5. Left prefix is [0] and right suffix is [].
>>Only one element, so child is a node with value 0.Empty array, so no child.
### Code
#### Method1 递归
>前序遍历构造二叉树，中处理逻辑为找到数组中的最大值，并创建节点。终止条件是当数组长度为1的时候终止，因为遍历到数组只有一个数了要创建node了。
>左右遍历加了if条件是为了不让空指针进入递归，终止条件在叶子节点。
```python
class Solution:
    def constructMaximumBinaryTree(self, nums: List[int]) -> Optional[TreeNode]:
        if len(nums) == 1:
            return TreeNode(nums[0])
        maxValue = max(nums)
        maxIndex = nums.index(maxValue)
        node = TreeNode(maxValue)
        
        if maxIndex>0:
            left = nums[:maxIndex]
            node.left = self.constructMaximumBinaryTree(left)
        if maxIndex<len(nums)-1:
            right = nums[maxIndex+1:]
            node.right = self.constructMaximumBinaryTree(right)
        return node
```
> - Time: O(N**2)
> - Space: O(N)
#### Method2 迭代
```python

```
> - Time: O(N)
> - Space: O(H)
## 617. Merge Two Binary Trees
[Leetcode Link](https://leetcode.cn/problems/merge-two-binary-trees/description/)-Easy
### Description
>You are given two binary trees root1 and root2.
>Imagine that when you put one of them to cover the other, some nodes of the two trees are overlapped while the others are not.
>You need to merge the two trees into a new binary tree.
>The merge rule is that if two nodes overlap, then sum node values up as the new value of the merged node. Otherwise, the NOT null node will be used as the node of the new tree.
>Return the merged tree.
>Note: The merging process must start from the root nodes of both trees.
>>**Example 1:**
>>**Input:**
>>root1 = [1,3,2,5], root2 = [2,1,3,null,4,null,7]
>>**Output:**
>>[3,4,5,5,4,null,7]
### Code
#### Method1 递归
>操作两棵二叉树，前序遍历即可，可以直接在一棵树上修改。因为两棵树是同时遍历，终止条件为一棵有一棵空，返回另一棵的节点。
>然后一起左右遍历。
```python
class Solution:
    def mergeTrees(self, root1: Optional[TreeNode], root2: Optional[TreeNode]) -> Optional[TreeNode]:
        return self.traversal(root1,root2)
    
    def traversal(self,node1,node2):
        if not node1:
            return node2
        if not node2:
            return node1
        node1.val += node2.val
        node1.left = self.traversal(node1.left,node2.left)
        node1.right = self.traversal(node1.right,node2.right)
        return node1
```
> - Time: O(N)
> - Space: O(H)
#### Method2 迭代
```python

```
> - Time: O(N)
> - Space: O(N)
## 700. Search in a Binary Search Tree
[Leetcode Link](https://leetcode.cn/problems/search-in-a-binary-search-tree/description/)-Easy
### Description
>You are given the root of a binary search tree (BST) and an integer val.
>Find the node in the BST that the node's value equals val and return the subtree rooted with that node. If such a node does not exist, return null.
>>**Example 1:**
>>**Input:**
>>root = [4,2,7,1,3], val = 2
>>**Output:**
>>[2,1,3]
### Code
#### Method1 递归
>二叉搜索树性质为左子树<根节点<右子树。
>前序遍历，终止条件为遍历到Null，或者节点值等于目标值的时候，直接返回根节点。
>利用性质，当根节点的值小于目标值，向右遍历，大于则向左遍历。
>将深层递归的值返回给上层必须写return！！！
```python
class Solution:
    def searchBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
        if not root or root.val == val:
            return root
        if val < root.val:
            return self.searchBST(root.left, val)
            #必须写return - 不是为了找到目标，而是为了把找到的结果传递回上层调用者。
            #如果不写，不会返回到上层。
        else:  # val > root.val
            return self.searchBST(root.right, val)
```
> - Time: O(LogN)
> - Space: O(H)
#### Method2 迭代
>迭代法直接返回root。
```python
class Solution:
    def searchBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
        if not root:
            return root
        while root:
            if val < root.val:
                root = root.left
            elif val > root.val:
                root = root.right
            else:
                return root
```
> - Time: O(LogN)
> - Space: O(H)
## 98. Validate Binary Search Tree
[Leetcode Link](https://leetcode.cn/problems/validate-binary-search-tree/description/)-Medium
### Description
>Given the root of a binary tree, determine if it is a valid binary search tree (BST).A valid BST is defined as follows:
>The left subtree of a node contains only nodes with keys less than the node's key
>The right subtree of a node contains only nodes with keys greater than the node's key.
>Both the left and right subtrees must also be binary search trees.
>>**Example 1:**
>>**Input:**
>>root = [2,1,3]
>>**Output:**
>>true
### Code
#### Method1 递归
>双指针出现，因为BST是一个有序数组，最好的方法就是中序遍历。
>定义一个空指针记录前一个节点。中终止条件为如果前面的指针不为空并且大于后面的指针return false。空返回true。
>pre = root就可以继续向后移动。
```python
class Solution:
    def __init__(self):
        self.pre = None  # 用来记录前一个节点

    def isValidBST(self, root):
        if root is None:
            return True

        left = self.isValidBST(root.left)

        if self.pre is not None and self.pre.val >= root.val:
            return False
        self.pre = root  # 记录前一个节点

        right = self.isValidBST(root.right)
        return left and right
```
> - Time: O(N)
> - Space: O(H)
#### Method2 迭代
```python

```
> - Time: O(N)
> - Space: O(H)
## 今日心得
- 一般情况来说：如果让空节点（空指针）进入递归，就不加if，如果不让空节点进入递归，就加if限制一下， 终止条件也会相应的调整。
- BST一般用中序遍历，可以创建前一个节点的双指针法。
- 如果不是单纯遍历，在深层递归有返回值，要么用一个变量接住，要么需要写return用来返回给上层，不然上层就不会有返回值，只有None。
