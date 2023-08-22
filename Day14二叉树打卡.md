---
title: Day14二叉树打卡
date: 2023-08-22 23:02:11
tags: lc打卡
---
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