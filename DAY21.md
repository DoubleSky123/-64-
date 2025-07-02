# DAY 21｜669. Trim a Binary Search Tree 108. Convert Sorted Array to Binary Search Tree 538. Convert BST to Greater Tree
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
## 669. Trim a Binary Search Tree
[Leetcode Link](https://leetcode.cn/problems/trim-a-binary-search-tree/description/)-Medium
### Description
>Given the root of a binary search tree and the lowest and highest boundaries as low and high, trim the tree so that all its elements lies in [low, high].
>Trimming the tree should not change the relative structure of the elements that will remain in the tree (i.e., any node's descendant should remain a descendant).
>It can be proven that there is a unique answer.
>Return the root of the trimmed binary search tree. Note that the root may change depending on the given bounds.
>>**Example 1:**
>>**Input:**
>>root = [1,0,2], low = 1, high = 2
>>**Output:**
>>[1,null,2]
### Code
#### Method1 递归
>BST性质，注意修剪时如果小于左边界，但需要返回右子树，可能右子树在边界内，同理大于右边界时返回左子树。
>最后用根节点接住返回的左右子树。
```python
class Solution:
    def trimBST(self, root: Optional[TreeNode], low: int, high: int) -> Optional[TreeNode]:
        if not root:
            return 
        if root.val <low:
            return self.trimBST(root.right,low,high)
        if root.val > high:
            return self.trimBST(root.left,low,high)
            
        root.left = self.trimBST(root.left,low,high)
        root.right = self.trimBST(root.right,low,high)
        return root
```
> - Time: O(N)
> - Space: O(H)
#### Method2 迭代
```python

```
> - Time: O(N)
> - Space: O(N)
## 108. Convert Sorted Array to Binary Search Tree
[Leetcode Link](https://leetcode.cn/problems/convert-sorted-array-to-binary-search-tree/description/)-Easy
### Description
>Given an integer array nums where the elements are sorted in ascending order, convert it to a height-balanced binary search tree.
>>**Example 1:**
>>**Input:**
>>nums = [-10,-3,0,5,9]
>>**Output:**
>>[0,-3,9,-10,null,5]
### Code
#### Method1 递归
>有序数组转化为高度平衡BST，只要每次取数组中间的值作为节点，就是高度平衡，即左右子树高度差不超过1.
```python
class Solution:
    def sortedArrayToBST(self, nums: List[int]) -> Optional[TreeNode]:
        if len(nums) == 0:
            return 
        mid = len(nums)//2
        node = TreeNode(nums[mid])
        node.left = self.sortedArrayToBST(nums[:mid])
        node.right = self.sortedArrayToBST(nums[mid+1:])
        return node
```
> - Time: O(N)
> - Space: O(N)
#### Method2 迭代
```python

```
> - Time: O(N)
> - Space: O(N)
## 538. Convert BST to Greater Tree
[Leetcode Link](https://leetcode.cn/problems/convert-bst-to-greater-tree/description/)-Medium
### Description
>Given the root of a Binary Search Tree (BST),
>convert it to a Greater Tree such that every key of the original BST is changed to the original key plus the sum of all keys greater than the original key in BST.
>As a reminder, a binary search tree is a tree that satisfies these constraints:
>The left subtree of a node contains only nodes with keys less than the node's key.
>The right subtree of a node contains only nodes with keys greater than the node's key.
>Both the left and right subtrees must also be binary search trees.
>>**Example 1:**
>>**Input:**
>>root = [4,1,6,0,2,5,7,null,null,null,3,null,null,null,8]
>>**Output:**
>>[30,36,21,36,35,26,15,null,null,null,33,null,null,null,8]
### Code
#### Method1 递归
> 右中左的遍历顺序，用双指针，pre一直等于前面一个节点的数值，用当前指针一直累加即可。主要是遍历顺序从右边开始，因为从最右边开始累加。
```python
class Solution:
    def __init__(self):
        self.pre = None

    def convertBST(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root:
            return 
        self.convertBST(root.right)
        if self.pre:
            root.val += self.pre.val
        self.pre = root
        self.convertBST(root.left)

        return root
```
> - Time: O(N)
> - Space: O(H)
#### Method2 迭代
```python

```
> - Time: O(N)
> - Space: O(H)
## 今日心得
- 二叉树终于结束了，有很多题型和技巧，比如求树的深度和高度，BST，以及双指针左中右的遍历。还有构造二叉树和二叉树中的增删。
- 最主要的先确定遍历顺序和返回值，有的时候只是需要遍历，但有的时候需要返回给上层，并且上层需要赋值接住。
