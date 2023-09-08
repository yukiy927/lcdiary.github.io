---
title: Day14二叉树打卡
date: 2023-08-22 23:02:11
tags: lc打卡
---
# 树与树算法（0823补充）

## 01 树的概念
- 每个节点有零个或多个子节点
- 没有父节点的节点称为根节点
- 每一个非根节点有且只有一个父节点
- 除了根节点之外，每个子节点可以分为多个不相交的子树

### 树的术语
- 节点的度
- 树的度
- 叶节点/终端节点
- 父节点
- 子节点
- 兄弟节点
- 节点的层次
- 树的高度/深度
- 堂兄弟节点
- 节点的祖先
- 子孙
- 森林

### 树的种类
- 无序树（没什么研究价值）
- 有序树
    - 二叉树
        - 满二叉树：除了叶节点外每一个节点都有左右子叶而且叶子节点都处在最底层的二叉树。
        - 完全二叉树：若设二叉树的高度为h，除第h层外，其他各层（1~h-1）的节点数都达到最大个数。第h层有叶子节点，并且叶节点都是从左到右依次排布。
        - 平衡二叉树（AVL树）：当且仅当任何节点的两颗子树的高度差不大于1的二叉树
        - 排序二叉树（二叉查找树，英文：Binary Search Tree）
    - 哈夫曼树（用于信息编码）
    - B树：一种对读写操作进行优化的自平衡的二叉查找树，能够保持数据有序，拥有多余两个字数。

### 树的存储与表示

顺序存储：将数据结构存储在固定的数组中，在遍历速度上有一定的优势，但是因为占空间比较大，是非主流二叉树。二叉树通常以链式存储。

链式存储：指针域数量随子节点数量变化，data-next-next

## 02 二叉树的概念
```python
>>>bool([])
False
>>>bool([None]) # 对列表本身判断，有元素就为真
True
```
## 03 二叉树的广度优先遍历(BFS)
二叉树的层次遍历
```python
def breadth_travel(self):
    """广度遍历"""
    if self.root is None:
        return
    queue = [self.root]
    while queue:
        cur_node = queue.pop(0)
        print(cur_node.elem)
        if cur_node.lchild is not None:
            queue.append(cur_node.lchild)
        if cur_node.rchild is not None:
            queue.append(cur_node.rchild)
```
## 04 二叉树的实现
```python
class Node(object):
    """节点构造"""
    def __init__(self, item):
        self.elem = item
        self.lchild = None
        self.rchild = None
    
class Tree(object):
    """二叉树"""
    def __init__(self):
        self.root = None

    def add(self, item):
        node = Node(item)
        if self.root is None:
            self.root = node
            return
        queue = [self.root]
        while queue:
            cur_node = queue.pop(0)
            if cur_node.lchild is None:
                cur_node.lchild = node
                return 
            else:
                queue.append(cur_node.lchild)
            if cur_node.rchild is None:
                cur_node.rchild = node
                return
            else:
                queue.append(cur_node.rchild)
```
## 05 二叉树的先序、中序、后序遍历
二叉树的**深度优先遍历（DFS）**
- 先序遍历preorder
    - 根节点->左子树->右子树
- 中序遍历inorder
    - 左子树->根节点->右子树
- 后序遍历postorder
    - 左子树->右子树->根节点
```python
def preorder(self, node):
    """先序遍历"""
    if node is None:
        return
    print(node.elem, end = " ")
    self.preorder(node.lchild)
    self.preorder(node.rchild)

def inorder(self, node):
    """中序遍历"""
    if node is None:
        return
    self.inorder(node.lchild)
    print(node.elem, end = " ")
    self.inorder(node.rchild)

def postorder(self, node):
    """后序遍历"""
    if node is None:
        return
    self.postorder(node.lchild)
    self.postorder(node.rchild)
    print(node.elem, end = " ")

```
## 06 二叉树由遍历确定一棵树

---------
--------- 
### Leetcode 144. 二叉树的前序遍历
题目链接：[144. 二叉树的前序遍历](https://leetcode.cn/problems/binary-tree-preorder-traversal/)
给你二叉树的根节点` root `，返回它节点值的 **前序** 遍历。

**思路**：没有思路，压根没懂，好烦

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def preorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        res = []

        if root is None:
            return []

        stack = [root]

        while stack:
            node = stack.pop()
            res.append(node.val)
            if node.right != None:
                stack.append(node.right)
            if node.left != None:
                stack.append(node.left)
        return res
```