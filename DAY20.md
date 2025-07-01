# DAY 20｜235. Lowest Common Ancestor of a Binary Search Tree 701. Insert into a Binary Search Tree 450. Delete Node in a BST
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
## 235. Lowest Common Ancestor of a Binary Search Tree
[Leetcode Link](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-search-tree/description/)-Medium
### Description
>Given a binary search tree (BST), find the lowest common ancestor (LCA) node of two given nodes in the BST.
>>**Example 1:**
>>**Input:**
>>root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
>>**Output:**
>>6
>>**Explanation:** The LCA of nodes 2 and 8 is 6.
### Code
#### Method1 递归
>BST的最近公共祖先一定是在给定两个值的中间，所以只要遍历到p，q的中间节点，即是最近公共祖先。
>因为遍历到某个节点需要返回给上层，所以左右遍历都需要返回值。
```python
class Solution:
    def lowestCommonAncestor(self, root, p, q):
        if root.val > p.val and root.val > q.val:
            return self.lowestCommonAncestor(root.left, p, q)
        elif root.val < p.val and root.val < q.val:
            return self.lowestCommonAncestor(root.right, p, q)
        else:
            return root
```
> - Time: O(N)
> - Space: O(N)
#### Method2 迭代
>BST性质，大于就向左遍历，小于就向右遍历，然后返回root。
```python
class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        while root:
            if root.val > p.val and root.val > q.val:
                root = root.left
            elif root.val < p.val and root.val < q.val:
                root = root.right 
            else:
                return root
        return None
```
> - Time: O(N)
> - Space: O(N)
## 701. Insert into a Binary Search Tree
[Leetcode Link](https://leetcode.cn/problems/insert-into-a-binary-search-tree/description/)-Medium
### Description
>You are given the root node of a binary search tree (BST) and a value to insert into the tree.
>Return the root node of the BST after the insertion. It is guaranteed that the new value does not exist in the original BST.
>Notice that there may exist multiple valid ways for the insertion, as long as the tree remains a BST after insertion. You can return any of them.
>>**Example 1:**
>>**Input:**
>>root = [4,2,7,1,3], val = 5
>>**Output:**
>>[4,2,7,1,3,5]
### Code
#### Method1 递归
>插入节点，只要插入在叶子节点即可，不会改变树形结构。
>同时大于就向左遍历，小于就向右遍历；遍历到空时说明可以插入了，从而返回新节点给上层。
>返回新的节点因为要成为叶子节点，所以要用root.left或者root.right接住。
```python
class Solution:
    def insertIntoBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
        if not root:
            node = TreeNode(val)
            return node
        
        if root.val > val:
            root.left = self.insertIntoBST(root.left,val)
        if root.val < val:
            root.right = self.insertIntoBST(root.right,val)
        
        return root
```
> - Time: O(H)
> - Space: O(H)
#### Method2 迭代
```python

```
> - Time: O(H)
> - Space: O(H)
## 450. Delete Node in a BST
[Leetcode Link](https://leetcode.cn/problems/delete-node-in-a-bst/description/)-Medium
### Description
>Given a root node reference of a BST and a key, delete the node with the given key in the BST. Return the root node reference (possibly updated) of the BST.
>Basically, the deletion can be divided into two stages:
>Search for a node to remove.
>If the node is found, delete the node.
>>**Example 1:**
>>**Input:**
>>root = [5,3,6,2,4,null,7], key = 3
>>**Output:**
>>[5,4,6,2,null,null,7]
### Code
#### Method1 递归
> 删除节点需要分情况：1.删除叶子节点，直接return None，将其返回给父节点
```python
class Solution:
    def deleteNode(self, root: Optional[TreeNode], key: int) -> Optional[TreeNode]:
        if not root:
            return None

        if root.val == key:
            if not root.left and not root.right:
                return None
            elif root.left and not root.right:
                return root.left
            elif root.right and not root.left:
                return root.right
            else:
                cur = root.right
                while cur.left:
                    cur = cur.left
                cur.left = root.left
                return root.right
        
        if root.val > key:
            root.left = self.deleteNode(root.left,key)
        if root.val < key:
            root.right = self.deleteNode(root.right,key) 
        return root
```
> - Time: O(H)
> - Space: O(H)
#### Method2 迭代
```python

```
> - Time: O(H)
> - Space: O(H)
## 今日心得
- BST题目需要活用双指针。
- 公共祖先用后序遍历，自下往上。
