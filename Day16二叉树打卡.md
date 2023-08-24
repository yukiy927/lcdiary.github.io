---
title: Day16二叉树打卡
date: 2023-08-24 22:37:26
tags: lc打卡
---
今日内容： 

● 104. 二叉树的最大深度  559. n叉树的最大深度

● 111. 二叉树的最小深度

● 222. 完全二叉树的节点个数

### Leetcode 104. 二叉树的最大深度
题目链接：[104. 二叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-binary-tree/)

Given the root of a binary tree, return *its maximum depth*.

A binary tree's **maximum depth** is the number of nodes along the longest path from the root node down to the farthest leaf node.

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        queue = deque([root])
        depth = 0
        if not root:
            return depth
        while queue:
            for _ in range(len(queue)):
                cur = queue.popleft()
                if cur.left:
                    queue.append(cur.left)
                if cur.right:
                    queue.append(cur.right)
            depth += 1
        return depth
```
### Leetcode 559. n叉树的最大深度
题目链接：[559. n叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-n-ary-tree/)

Given a n-ary tree, find its maximum depth.

The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

Nary-Tree input serialization is represented in their level order traversal, each group of children is separated by the null value (See examples).

```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val=None, children=None):
        self.val = val
        self.children = children
"""
from collections import deque
class Solution:
    def maxDepth(self, root: 'Node') -> int:
        queue = deque([root])
        depth = 0
        if not root:
            return depth
        while queue:
            for _ in range(len(queue)):
                cur = queue.popleft()
                for child in cur.children:
                    queue.append(child)
            depth += 1
        return depth
```
### Leetcode 111. 二叉树的最小深度
题目链接：[111. 二叉树的最小深度](https://leetcode.cn/problems/minimum-depth-of-binary-tree/)

Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

Note: A leaf is a node with no children.

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def minDepth(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        queue = deque([root])
        depth = 0
        while queue:
            for _ in range(len(queue)):
                cur = queue.popleft()
                if cur.left is None and cur.right is None:
                    return depth + 1
                if cur.left:
                    queue.append(cur.left)
                if cur.right:
                    queue.append(cur.right)
            depth += 1
        return depth
```
### Leetcode 222. 完全二叉树的节点个数
题目链接：[222. 完全二叉树的节点个数](https://leetcode.cn/problems/count-complete-tree-nodes/)

Given the root of a complete binary tree, return the number of the nodes in the tree.

According to Wikipedia, every level, except possibly the last, is completely filled in a complete binary tree, and all nodes in the last level are as far left as possible. It can have between 1 and 2h nodes inclusive at the last level h.

Design an algorithm that runs in less than O(n) time complexity.

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def countNodes(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        queue = deque([root])
        cnt = 1
        while queue:
            for _ in range(len(queue)):
                cur = queue.popleft()
                if cur.left:
                    queue.append(cur.left)
                    cnt += 1
                if cur.right:
                    queue.append(cur.right)
                    cnt += 1
        return cnt
```
