---
title: Day18二叉树打卡
date: 2023-08-26 20:54:25
tags: lc打卡
---
今日内容 

● 513. 找树左下角的值

● 112. 路径总和  113. 路径总和ii

● 106. 从中序与后序遍历序列构造二叉树 105. 从前序与中序遍历序列构造二叉树

### Leetcode 513. 找树左下角的值
题目链接：[513. 找树左下角的值](https://leetcode.cn/problems/find-bottom-left-tree-value/)

给定一个二叉树的 根节点 root，请找出该二叉树的 最底层 最左边 节点的值。

假设二叉树中至少有一个节点。

**思路**：这道题需要找的是最底层的最左边，两个限定条件：
1. 最底层
2. 最左边

因此，可以用**层序遍历**，找到最底层，记录最底层的兄弟节点，返回第一个节点即可。

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def findBottomLeftValue(self, root: Optional[TreeNode]) -> int:
        queue = deque([root])
        while queue:
            res = []
            for _ in range(len(queue)):
                cur = queue.popleft()
                res.append(cur.val)
                if cur.left:
                    queue.append(cur.left)
                if cur.right:
                    queue.append(cur.right)
        return res[0]
```
### Leetcode 112. 路径总和
题目链接：[112. 路径总和](https://leetcode.cn/problems/path-sum/)

给你二叉树的根节点 root 和一个表示目标和的整数 targetSum 。判断该树中是否存在 根节点到叶子节点 的路径，这条路径上所有节点值相加等于目标和 targetSum 。如果存在，返回 true ；否则，返回 false 。

叶子节点 是指没有子节点的节点。

**思路**：
递归函数什么时候要有返回值，什么时候没有返回值，特别是有的时候递归函数返回类型为bool类型。

#### 112和113是姊妹题，可以深度探讨递归函数返回值的问题：

- 如果需要搜索整棵二叉树且不用处理递归返回值，递归函数就不要返回值。（这种情况就是本文下半部分介绍的113.路径总和ii）
- 如果需要搜索整棵二叉树且需要处理递归返回值，递归函数就需要返回值。 （这种情况我们在236. 二- 叉树的最近公共祖先 (opens new window)中介绍）
- 如果要搜索其中一条符合条件的路径，那么递归一定需要返回值，因为遇到符合条件的路径了就要及时返回。（本题的情况）

#### 递归函数三步骤：
1. 确定递归函数的参数和返回类型
2. 确定终止条件
3. 确定单层递归的逻辑

#### 本题思路：
1. 因为找到一条路径就可以返回true了，递归必须得有返回值，返回类型为bool。这里参数添加一个计数器cnt，初始值设为target，每遇到一个节点就--其val。
2. 当叶子节点到达，且计数值为0，则说明找到一条，返回true；否则返回false。
3. 左遍历，计数--，递归，回溯；右遍历同理。

方法一，回溯细节写法：
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def hasPathSum(self, root: Optional[TreeNode], targetSum: int) -> bool:
        def traversal(node: TreeNode, cnt: int) -> bool:
            
            if node.left is None and node.right is None and cnt == 0:
                return True
            if node.left is None and node.right is None:
                return False

            if node.left:
                cnt -= node.left.val
                if traversal(node.left, cnt) == True:   # 递归
                    return True
                cnt += node.left.val    # 回溯
            if node.right:
                cnt -= node.right.val
                if traversal(node.right, cnt) == True:  # 递归
                    return True
                cnt += node.right.val   # 回溯

            return False
            
        if not root:
                return False
        return traversal(root, targetSum - root.val)
```
方法二，回溯隐藏写法：
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def traversal(self, node: TreeNode, cnt: int) -> bool: 
            if node.left is None and node.right is None and cnt == 0:
                return True
            if node.left is None and node.right is None:
                return False

            if node.left: # 左
                if self.traversal(node.left, cnt-node.left.val) == True:    # 回溯
                    return True
            if node.right:  # 右
                if self.traversal(node.right, cnt-node.right.val) == True:   # 回溯
                    return True

            return False

    def hasPathSum(self, root: Optional[TreeNode], targetSum: int) -> bool:
        if not root:
                return False
        return self.traversal(root, targetSum - root.val)
```

### Leetcode 113. 路径总和 II
题目链接：[113. 路径总和 II](https://leetcode.cn/problems/path-sum-ii/)

给你二叉树的根节点 root 和一个整数目标和 targetSum ，找出所有 从根节点到叶子节点 路径总和等于给定目标和的路径。

叶子节点 是指没有子节点的节点。

#### 本题思路：
1. 因为需要找出所有路径，递归不需要返回值，返回类型为void。这里参数添加一个计数器cnt，初始值设为target，每遇到一个节点就--其val。但是此时需要传参，这里定义一个全局变量result和path。
2. 当叶子节点到达，且计数值为0，则说明找到一条，把当前path添加到result里，注意是列表嵌套模式；否则返回空。
3. 左遍历，计数--，递归，回溯；右遍历同理。

代码如下，回溯写的很清楚版本：
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def __init__(self):
        # 传递这两个变量
        self.result = []
        self.path = []

    def traversal(self, node: TreeNode, cnt: int):
        # 需要遍历所有可能性，无需递归返回值，void
        if not node.left and not node.right and cnt == 0:          # 递归的终止条件
            # 到达叶子节点，并且和符合要求
            self.result.append(self.path[:])
            return
        if node.left is None and node.right is None:
            # 到达叶子节点，但是不符合要求，直接返回
            return

        if node.left:   # 左（空节点不遍历）
            self.path.append(node.left.val)
            cnt -= node.left.val
            self.traversal(node.left, cnt)  # 递归
            cnt += node.left.val   # 回溯
            self.path.pop() # 回溯

        if node.right:  # 右（空节点不遍历）
            self.path.append(node.right.val)
            cnt -= node.right.val
            self.traversal(node.right, cnt) # 递归
            cnt += node.right.val   # 回溯
            self.path.pop() # 回溯

        return

    def pathSum(self, root: Optional[TreeNode], targetSum: int) -> List[List[int]]:
        self.result.clear()
        self.path.clear()
        if not root:
            return []
        self.path.append(root.val)  # 把根节点放进路径
        self.traversal(root, targetSum-root.val)
        return self.result
```

### Leetcode 106. 从中序与后序遍历序列构造二叉树
题目链接：[106. 从中序与后序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

给定两个整数数组 inorder 和 postorder ，其中 inorder 是二叉树的中序遍历， postorder 是同一棵树的后序遍历，请你构造并返回这颗 二叉树 。

**思路**：首先，要明确这个搭建的本身逻辑，中序必不可少，因为它用来区分左右，至于后序和前序，有一个就可以奠定根节点的位置了。有了根节点，中序才能区分左右。

来看一下一共分几步：

第一步：如果数组大小为零的话，说明是空节点了。

第二步：如果不为空，那么取后序数组最后一个元素作为节点元素。

第三步：找到后序数组最后一个元素在中序数组的位置，作为切割点

第四步：切割中序数组，切成中序左数组和中序右数组 （顺序别搞反了，一定是先切中序数组）

第五步：切割后序数组，切成后序左数组和后序右数组

第六步：递归处理左区间和右区间

代码如下：
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def buildTree(self, inorder: List[int], postorder: List[int]) -> Optional[TreeNode]:
        # 第一步：特殊情况讨论，树为空（递归终止条件）
        if not postorder:
            return None
        
        # 第二步：后序遍历的最后一个就是当前的中间节点
        root_val = postorder[-1]
        root = TreeNode(root_val)

        # 第三步：找切割点
        separator_idx = inorder.index(root_val)

        # 第四步：切割inorder数组，得到inorder的左右半边
        inorder_left = inorder[:separator_idx]
        inorder_right = inorder[separator_idx + 1:]

        # 第五步：切割postorder数组，得到postorder的左右半边
        # 重点是：中序数组的大小一定和后序数组大小相等
        postorder_left = postorder[:len(inorder_left)]
        postorder_right = postorder[len(inorder_left):len(postorder)-1]

        # 第六步：递归
        root.left = self.buildTree(inorder_left, postorder_left)
        root.right = self.buildTree(inorder_right, postorder_right)

        # 第七步：返回答案
        return root
```

### Leetcode 105. 从前序与中序遍历序列构造二叉树
题目链接：[105. 从前序与中序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

给定两个整数数组 preorder 和 inorder ，其中 preorder 是二叉树的先序遍历， inorder 是同一棵树的中序遍历，请构造二叉树并返回其根节点。

**思路**：这个地方注意一下前序和后序数组的切割写法不一样，但是比较简单，想想就清楚了。

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def buildTree(self, preorder: List[int], inorder: List[int]) -> Optional[TreeNode]:
        # 第一步：特殊情况讨论，树为空（递归终止条件）
        if not preorder:
            return None
        
        # 第二步：前序遍历的第一个就是当前的中间节点
        root_val = preorder[0]
        root = TreeNode(root_val)

        # 第三步：找切割点
        separator_idx = inorder.index(root_val)

        # 第四步：切割inorder数组，得到inorder的左右半边
        inorder_left = inorder[:separator_idx]
        inorder_right = inorder[separator_idx+1:]

        # 第五步：切割preorder数组，得到preorder的左右半边
        # 重点是：中序数组的大小一定和前序数组大小相等
        preorder_left = preorder[1:len(inorder_left)+1]
        preorder_right = preorder[len(inorder_left)+1:]

        # 第六步：递归
        root.left = self.buildTree(preorder_left, inorder_left)
        root.right = self.buildTree(preorder_right, inorder_right)

        # 第七步：返回答案
        return root
```

### 总结
最近好疲惫，不想写，尤其是二叉树，深度遍历真的好绕，经常写的自己都迷糊了。

美团收到一面了，还得好好准备。八股文+自我介绍+项目剖析，就这样吧。

国内外找工都有队友了，可是9月4号就要走了，好烦啊，真想继续呆在家里。