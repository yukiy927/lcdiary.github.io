---
title: Day06哈希表打卡
date: 2023-08-14 07:47:04
tags: lc打卡
---
### **一般哈希表都是用来快速判断一个元素是否出现集合里。**

今天到哈希表啦，静下心来看书发现原来并不难，但是之前就是看不进去，今天都是一刷，前两道题自己就写出来啦，挺有感觉的。

看了估计一两个小时的书吧，哈希表底层逻辑还是不懂的，但是知道每个语言都有现成的函数给你写，python里字典就是个很好的助手。哈希表可以帮助你**缓存**，**防止重复**，哈希**查找**。


## Leetcode 242 有效的字母异位词

题目链接：[242. 有效的字母异位词](https://leetcode.com/problems/valid-anagram/description/)

Given two strings s and t, return true if t is an anagram of s, and false otherwise.

An Anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

<!-- ![swap_ex1](https://github.com/yukiy927/lcdiary.github.io/blob/main/pics/swap_ex1.jpg) -->

**思路**：因为题目有说s和t只含有小写字母，所以就造了两个字典，分别用来对s和t中出现的字母计数，然后对比两个字典是否一致。

最初写的一个版本没有过测试，因为我在创建第二个字典的时候，偷懒直接写dtt=dts，导致后面dts每次更新都会带着dtt一起，他们永远相等，这一点要注意。

然后拓展中有写道，如果不只是字母怎么办，unicode怎么办，这些问题我的代码目前都不能给到很好的解答。

代码如下：

```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        dts = dict()
        dtt = dict()

        dts = {'a':0, 'b':0, 'c':0, 'd':0, 'e':0,
        'f':0, 'g':0, 'h':0, 'i':0, 'j':0, 'k':0,
        'l':0, 'm':0, 'n':0, 'o':0, 'p':0, 'q':0,
        'r':0, 's':0, 't':0, 'u':0, 'v':0, 'w':0,
        'x':0, 'y':0, 'z': 0}
        dtt = {'a':0, 'b':0, 'c':0, 'd':0, 'e':0,
        'f':0, 'g':0, 'h':0, 'i':0, 'j':0, 'k':0,
        'l':0, 'm':0, 'n':0, 'o':0, 'p':0, 'q':0,
        'r':0, 's':0, 't':0, 'u':0, 'v':0, 'w':0,
        'x':0, 'y':0, 'z': 0}

        for c in s:
            dts[c] += 1
        
        for c in t:
            dtt[c] += 1

        if dts == dtt:
            return True

        else:
            return False
```

## Leetcode 349 两个数组的交集 
题目链接：[349. 两个数组的交集 ](https://leetcode.com/problems/intersection-of-two-arrays/description/)

Given two integer arrays nums1 and nums2, return an array of their intersection. Each element in the result must be unique and you may return the result in any order.

**思路**：创了一个字典，用来记录第一个数组里的数据，key是数据，value都记为0。然后遍历第二个数组，投射到字典里，如果已经存在在字典了，key对应的value就+1，这样返回不等于0的key就好了。其实本质是用到了哈希表的防止重复的思想。


代码如下：
```python
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        dt = dict()
        for i in nums1:
            if i not in dt:
                dt[i] = 0
            else:   pass
        
        for i in nums2:
            if i in dt:
                dt[i] += 1
            else:   pass
        
        return [i for i in dt if dt[i] != 0]
```

## Leetcode 202 快乐数 
题目链接：[202. 快乐数](https://leetcode.com/problems/happy-number/description/)

Write an algorithm to determine if a number `n` is happy.

A happy number is a number defined by the following process:

- Starting with any positive integer, replace the number by the sum of the squares of its digits.
- Repeat the process until the number equals 1 (where it will stay), or it **loops endlessly in a cycle** which does not include 1.
- Those numbers for which this process ends in 1 are happy.

Return `true` if `n` is a happy number, and `false` if not.


**思路**：这道题没有很仔细的看题，我以为是在考我数学，我心想这一时半会怎么可能证明的出来，搜了一下快乐数的性质，发现回不到1的数字都会进入循环，然后根据循环判断就好了。

但是后来发现这个题目里面已经写的很清楚了，加粗部分，会陷入无尽循环，因此可以用着一个特性，同样是哈希表可以防止重复，一旦重复了就跳出循环，但是这个时候有可能一直在求和为1，所以在返回那里需要加个条件判断。

代码如下：
```python
class Solution:
    def isHappy(self, n: int) -> bool:
        # 十进位不快乐数的循环只有一个
        # 4 → 16 → 37 → 58 → 89 → 145 → 42 → 20 → 4
        a = []
        while n not in a:
            a.append(n)
            n = sum(int(x)**2 for x in str(n))
        
        return True if n == 1 else False 
```


## Leetcode 1 两数之和 
题目链接：[1. 两数之和](https://leetcode.com/problems/two-sum/description/)

Given an array of integers `nums` and an integer `target`, return *indices of the two numbers such that they add up to* `target`.

You may assume that each input would have ***exactly*** **one solution**, and you may not use the same element twice.

You can return the answer in any order.

**思路**：这道题我只想到暴力怎么写了，也过了，2k多ms，呜呜。

方法一：暴力O(n^2)
```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        for i in range(len(nums)):
            # if nums[i] <= target:
            for j in range(i+1, len(nums)):
                    # if nums[j] <= target:
                if nums[i] + nums[j] == target:
                    return [i, j]
        return None
```
方法二：用map，其实我还没搞明白c++里面map和set和数组都分得很开，在python里面有没有区别。

这道题要考虑，

1. 首先为什么用到map：

    - 因为这个时候你不仅要记住数组里的数，还要记住他们的下标，而set和数组都记不了两个，但是map可以，所以我们一定要用map
2. 怎么用map呢：
    - 对出现过的数字，采用一找一的方法，没找到就存到字典里，这样利用字典的O(1)加上只需要存一遍，就可以达到O(n)的效果。
```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        a = dict()
        for i in range(len(nums)):
            if target - nums[i] not in a:
                a[nums[i]] = i
            else:
                return [i, a[target-nums[i]]]
        return None
```
## 总结

时长：一个半小时哈希表学习+代码一个小时

今天晚上有点事情，就没有细究很多东西，其实每道题我都有很多小问题，比如：
- 再写一遍滑动窗口
- 总结数组，set集合，list，字典的创建及基本内建函数
- 总结哈希表的基本结构
- Unicode字符和utf-8有什么区别