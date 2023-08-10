---
title: My New Post
date: 2023-08-09 07:39:58
tags:
---
## Leetcode 704 二分查找
题目链接：[704. 二分查找](https://leetcode.com/problems/binary-search/)

Given an array of integers nums which is sorted in ascending order, and an integer target, write a function to search target in nums. If target exists, then return its index. Otherwise, return -1.

You must write an algorithm with O(log n) runtime complexity.

**思路**：binary search，我比较习惯写的是左闭右开，区间开闭是二分写法的核心，这里主要影响到两个写代码时候的核心要点

1. while的判断条件
    - 二刷的时候想到一个巧思，可以代入区间的数学性质，左闭右开只有在左边小于右边的情况下，区间才有意义，循环也就才能继续
    - 这里拓展到另一个写法——左闭右闭，这时候左右相等区间也是有意义的
    - 左闭右开：`while left < right`
    - 左闭右闭：`while left <= right`

2. 每轮循环结束后，右区间的更新
    - 左闭右开：说明right本身不会考虑，因为mid已经判断过了，所以直接`right = mid`即可
    - 左闭右闭：此时由于是闭区间，right本身依然要考虑，然而mid已经判断过了，所以此时要跳过mid，即`right = mid - 1`

代码如下：

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        right = len(nums)
        left = 0

        while left < right:
            mid = (right + left) // 2
            if nums[mid] == target:
                return mid
            elif nums[mid] < target:
                left = mid + 1
            else:
                right = mid
            
        return -1
```

## Leetcode 27 移除元素
题目链接：[27. 移除元素](https://leetcode.com/problems/remove-element/)

Given an integer array nums and an integer val, remove all occurrences of val in nums <u>**in-place**</u>. The order of the elements may be changed. Then return the number of elements in nums which are not equal to val.

Consider the number of elements in nums which are not equal to val be k, to get accepted, you need to do the following things:

Change the array nums such that the first k elements of nums contain the elements which are not equal to val. The remaining elements of nums are not important as well as the size of nums.
Return k.

**思路**：Carl哥提供了两个解法，暴力和双指针，没想到二刷的时候只会写双指针了…这里题目里有个词第一次见，用黑体标出来了，in-place，<u>在原有数组、list或者其他数据结构上，直接修改里面的内容，而不通过其他数据结构辅助修改。</u>好处就是没有extra memory，所以对这部分数据的修改，做到了O(1) space。

1. 双指针 Two Pointers：

    先写我自己熟悉的吧，本质是快慢指针，用慢指针存取与val不等的元素，并且最后靠pre计数返回长度，用快指针进行过筛，专挑和val不相等的元素筛选。

    所以while的判断条件就是只要快指针没溢出即可。

    代码如下：
    ```python
    class Solution:
        def removeElement(self, nums: List[int], val: int) -> int:
            pre, cur = 0, 0
            k = 0
            while cur < len(nums):
                if nums[cur] != val:
                    nums[pre], nums[cur] = nums[cur], nums[pre]
                    pre += 1
                    cur += 1
                    k += 1
        
                else:
                    cur += 1

            return k
    ```


2. 暴力 Brute Force：

    看了眼Carl哥的手册才写出来qwq，第二个for循环那里把 `i--` 想了半天，才明白是为了抵消每层的 `i++`，挺合理的。核心思想是第一层for过筛nums里的元素，这里和第一种方法不同的是，专找与val相等的元素，然后用第二层for把该元素后的所有数值往前面进一个，覆盖掉当前位。

    代码如下：
    ```python
    class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        # brute force
        i, l = 0, len(nums)

        while i < l:
            if nums[i] == val:
                for j in range(i+1, l):
                    nums[j-1] = nums[j]
                i -= 1
                l -= 1

            i += 1

        return l
    ```