---
title: Day13栈与队列打卡
date: 2023-08-21 16:58:54
tags: lc打卡
---
### Leetcode 239. 滑动窗口最大值
题目链接：[239. 滑动窗口最大值](https://leetcode.com/problems/sliding-window-maximum/)

You are given an array of integers nums, there is a sliding window of size k which is moving from the very left of the array to the very right. You can only see the k numbers in the window. Each time the sliding window moves right by one position.

Return the max sliding window.

**思路**：第一道Hard，暴力过了72%，但是方法不够好，O(n*m)的时间复杂度，维护了所有元素，包括不重要的不相关的元素。

这道题的主要思想是构造一个单调队列，这个单调队列能够保证头部永远存的是最大值，尾部永远存着最小值。避免了暴力解法中max函数的时间复杂度，而是直接返回队列的头部得到res序列。

![单调队列](https://github.com/yukiy927/lcdiary.github.io/blob/main/pics/3c1114530895b8007c4f440b99ebff5.png)

单调队列：
1. 首先利用deque，因为list的pop(0)没有popleft时间复杂度小
2. `def pop()`：如果要出去的数刚好等于队列的头部元素，那就把它popleft出去，否则就不管，留在队列中
3. `def push()`：push涉及到如何构造这个单调队列：
    - 首先如果要进来的数比队尾大，那就把队尾一直pop
    - 直到要进来的这个数比队尾小或等于，再把它append到队尾，保证队列是从大到小的
    - 如果要进来的数本身就比队尾小，直接加到队尾
4. `def front()`：返回队头就好了

而暴力的解法在这里是，同利用了双向队列deque，但是遍历整个数组时，把每一组滑动窗口的元素全部存在队列中，就很浪费时空。

但是这道题我觉得和美团笔试后几题异曲同工之处就在于，暴力思想很简单，但是不知道怎么优化，想不出优化算法。

单调队列代码如下：
```python
from collections import deque

class MyQueue:  # 单调队列，[0]放最大，[-1]最小
    def __init__(self):
        self.dq = deque()
    
    def pop(self, value):
        # 如果下一个窗口要pop的是[0]，就pop，否则不管
        if self.dq and value == self.dq[0]:
            self.dq.popleft()

    def push(self, value):
        # 如果下一个窗口要加的数比[-1]小，就加进来，如果大，就把比他小的都pop掉，再加进来，保证单调性。
        while self.dq and value > self.dq[-1]:
            self.dq.pop()
        self.dq.append(value)
    
    def front(self):
        return self.dq[0]

class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        dq = MyQueue()
        res = []
        # 先存进来最前面k个
        for i in range(k):
            dq.push(nums[i])
        
        res.append(dq.front())
        # i指向滑动窗口的最后一个
        for i in range(k, len(nums)):
            dq.pop(nums[i-k])   #滑动窗口移除最前面元素
            dq.push(nums[i])    #滑动窗口前加入最后面的元素
            res.append(dq.front())
        
        return res
```

暴力代码如下70%：
```python
class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        dq = deque()
        res = []
        for i in range(len(nums)-k+1):
            if i == 0:
                for l in range(k):
                    dq.append(nums[i+l])
            else:
                dq.popleft()
                dq.append(nums[i+k-1])
            res.append(max(dq))

        return res
```