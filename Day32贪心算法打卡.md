---
title: Day32贪心算法打卡
date: 2023-09-08 16:04:29
tags: lc打卡
---
● 122.买卖股票的最佳时机II 
● 55. 跳跃游戏 
● 45.跳跃游戏II 

### Leetcode 122.买卖股票的最佳时机II
题目链接：[122.买卖股票的最佳时机II](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/)

给你一个整数数组 prices ，其中 prices[i] 表示某支股票第 i 天的价格。

在每一天，你可以决定是否购买和/或出售股票。你在任何时候 最多 只能持有 一股 股票。你也可以先购买，然后在 同一天 出售。

返回 你能获得的 最大 利润 。

**思路：**
注意审题，题目的意思是，手上仅能留一只股，也就是即买即抛，所以交易单元由两天完成。

这种情况下，利润相加就等于price[1]-price[0]+price[2]-price[1]+...，拆分来看就发现其实是由前后两天的差值组成的，那么我们只需要把两两之间的差求出来，取其中的正数值求和就能得到最大利润了。由于题目中没有要求返回具体的交易方式，因此这样就可以了。

**局部最优：每两天之间利润为正的交易**

**全局最优：所有正利润的和就是最大利润**

代码如下：
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        # 手头上只能存一只股，所以想买先得卖
        res = 0
        for i in range(1, len(prices)):
            res += max(prices[i]-prices[i-1], 0)    # 利用max函数和0比较得到正利润
        return res
```

### Leetcode 55. 跳跃游戏 
题目链接：[55. 跳跃游戏 ](https://leetcode.cn/problems/jump-game/)

给你一个非负整数数组 nums ，你最初位于数组的 第一个下标 。数组中的每个元素代表你在该位置可以跳跃的最大长度。

判断你是否能够到达最后一个下标，如果可以，返回 true ；否则，返回 false 。

**思路：**
利用**区域覆盖**的想法，利用循环在可覆盖区域内前进游走，然后不断更新当前位置的新的可覆盖范围，这里范围直接用可覆盖的最后一个元素的下标表示，这时候如果可以覆盖到最后一个元素，即返回真。

代码如下：
```python
class Solution:
    def canJump(self, nums: List[int]) -> bool:
        cover = 0
        # if the size of array is 1, return true
        if len(nums) == 1:  return True
        # update the maximum coverage
        # rmb: for loop cannot dinamycally change variable
        # for i in range(0, cover+1):
        i = 0
        while i <= cover:
            cover = max(i+nums[i], cover)
            if cover >= len(nums) - 1:  return True
            i += 1
        return False
```

### Leetcode 45.跳跃游戏II 
题目链接：[45.跳跃游戏II ](https://leetcode.cn/problems/jump-game-ii/)

给定一个长度为 n 的 0 索引整数数组 nums。初始位置为 nums[0]。

每个元素 nums[i] 表示从索引 i 向前跳转的最大长度。换句话说，如果你在 nums[i] 处，你可以跳转到任意 nums[i + j] 处:

```0 <= j <= nums[i] ```

```i + j < n```

返回到达 nums[n - 1] 的最小跳跃次数。生成的测试用例可以到达 nums[n - 1]。

**思路：**
这题要求得到最小跳跃次数，这里在原本的while条件里多加了一层for循环，是为了实现每次都走最大的一步，以达到最小跳跃次数的要求。

如何实现的呢：

在当前的覆盖区域之下，遍历该区域里可到达位置的新的覆盖区域，一旦能够覆盖到末尾元素，就返回计数器+1的值，因为这一步是必不可免要走的。

如果这一整个覆盖区域都无法使下一覆盖区域覆盖到末尾元素，计数器+1，进入下一个最长的覆盖区域，这样可以减少外层while的循环次数，并且也能确保走的是最大步伐。

代码如下：
```python
class Solution:
    def jump(self, nums: List[int]) -> int:
        if len(nums) == 1: 
            return 0 # no need jump

        current_cover = 0 # current cover
        # next_cover = 0  # saved next cover
        cnt = 0 # step counter
        i = 0
        
        while i <= current_cover:
            for i in range(i, current_cover + 1):
                current_cover = max(i+nums[i], current_cover)
                if current_cover >= len(nums)-1:    
                    # if cover to the end
                    return cnt + 1
            cnt += 1
```

### 总结
好久没刷题了，落了很多天，9.4又飞回la了，现已安顿，希望开口前先想想，多练习表达，心静下来。