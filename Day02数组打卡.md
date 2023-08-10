---
title: Day02数组打卡
date: 2023-08-10 08:50:02
tags: lc打卡
---
## Leetcode 977 有序数组的平方

题目链接：[977. 有序数组的平方](https://leetcode.cn/problems/squares-of-a-sorted-array/)

Given an integer array nums sorted in non-decreasing order, return an array of the squares of each number sorted in non-decreasing order.

**思路**：今天太晚了，浅浅写一下，后面有机会再补吧

方法一：双指针（没想出来）

    关键在于，想逻辑而不是代码本身了，虽然我代码现阶段水平还是不行，但是这题逻辑更重要，逻辑点在于，平方了之后，最大的都在两边，这时候可以双指针比较两边取最大值保存到新数组里。

代码如下：

```python
class Solution:
    def sortedSquares(self, nums: List[int]) -> List[int]:
        # Two Pointers

        i = 0
        j = len(nums) - 1
        result = [float('inf')] * len(nums)
        k = len(nums) - 1
        while i <= j:
            if nums[i]**2 >= nums[j]**2:
                result[k] = nums[i]**2
                i += 1
            else:
                result[k] = nums[j]**2
                j -= 1 
            
            k -= 1

        return result
```
方法二：暴力（自己写出来的唯一一题）

    暴力很简单咯，for先把平方写好，然后sorted，这题暴力更快诶好像（？待求证）。

代码如下：

```python
class Solution:
    def sortedSquares(self, nums: List[int]) -> List[int]:
        # brute force
        if nums is None:
            return nums
        for i in range(len(nums)):
            nums[i] = nums[i]**2
            
        return sorted(nums)
```
## Leetcode 209 长度最小的子数组
题目链接：[209. 长度最小的子数组](https://leetcode.cn/problems/minimum-size-subarray-sum/)

Given an array of positive integers nums and a positive integer target, return the minimal length of a subarray whose sum is greater than or equal to target. If there is no such subarray, return 0 instead.

**思路**：滑动窗口

    完全不知道怎么写，这还是二刷，双指针完成的滑动窗口，遍历的是终止位置，起始位置在终止位置确定了之后再定。

    Tips：这里有个要点就是while里的前后顺序，一定是先记下subl和result，然后再更新sums和i的起始位置。

代码如下：
```python
class Solution:
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        # 滑动窗口 Sliding Window Algorithm
        result = float('inf')
        i = 0
        sums = 0
        for j in range(0, len(nums)):
            sums += nums[j]
            
            while sums >= target:
                subl = j - i + 1
                result = min(subl, result)
                sums -= nums[i]
                i += 1
                
                
        if result == float('inf'):
            return 0
        else:
            return result
```

## Leetcode 59 螺旋矩阵 II
题目链接：[59. 螺旋矩阵 II](https://leetcode.cn/problems/spiral-matrix-ii/)

Given a positive integer n, generate an n x n matrix filled with elements from 1 to n2 in spiral order.

**思路**：模拟

模拟出螺旋矩阵的更新法则，说白了就是找规律，这题好像很常考，规律一直记得，但是代码实现能力跟不上自己的找规律能力，只能说这个就得多写了，位置更新，循环更新，起始终止位置，位移长度，特殊位置（奇数的时候正中间的位置），都要考虑到，55继续加油吧TT

代码如下：
```python
class Solution:
    def generateMatrix(self, n: int) -> List[List[int]]:
        startx = 0
        starty = 0
        offset = 1
        loop = n // 2
        mid = n // 2
        cnt = 1
        matrix = [[0]*n for _ in range(n)]

        while loop:
            for i in range(starty, n - offset):
                matrix[startx][i] = cnt
                cnt += 1
            for i in range(startx, n - offset):
                matrix[i][n-offset] = cnt
                cnt += 1
            for i in range(n-offset, starty, -1):
                matrix[n-offset][i] = cnt
                cnt += 1
            for i in range(n-offset, startx, -1):
                matrix[i][starty] = cnt 
                cnt += 1
            loop -= 1
            offset += 1
            startx += 1
            starty += 1

        if n % 2 == 1:
            matrix[mid][mid] = cnt
        
        return matrix
```

总结一下，这一期没有一题是自己写出来的，这还是二刷，我真无语，然后从明天开始不能等到晚上再开始了，时间很明显不够，尤其是加上写博客的时间，不喜欢熬夜，警告一下。