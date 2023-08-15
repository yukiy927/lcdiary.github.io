---
title: Day07哈希表打卡
date: 2023-08-15 08:40:39
tags: lc打卡
---
● 454.四数相加II 
● 383. 赎金信 
● 15. 三数之和 
● 18. 四数之和


15和18是挺好的题，值得2刷。


## Leetcode 454 四数相加II 
题目链接：[454. 四数相加II](https://leetcode.com/problems/4sum-ii/description/)

Given four integer arrays `nums1`, `nums2`, `nums3`, and `nums4` all of length `n`, return the number of tuples `(i, j, k, l)` such that:

    0 <= i, j, k, l < n

    nums1[i] + nums2[j] + nums3[k] + nums4[l] == 0

<!-- ![swap_ex1](https://github.com/yukiy927/lcdiary.github.io/blob/main/pics/swap_ex1.jpg) -->

**思路**：以下是我自己的思路，也是方法一
1. 建造两个map，分别是数组一和数组二的遍历和集合，以及数组三和数组四的遍历和集合，如果出现相等的和，value位就往上计数。
2. 然后遍历其中一个map，查找另一个map中有没有-(a+b)的值，有的话，就对计数器加上二者对应value相乘的乘积，这一点是利用了排列组合的原理，因为只要返回计数即可，所以只在乎计数值就好了。

四数相加这题虽然是medium，但
- 基于考虑对象是四个独立的数组，也就是说，可操作空间大多了；
- 题目对于重复元素没有提出要求，找到就+1即可；
- 题目对于下标也没有要求，`i, j, k, l`可以自由相等；
- 不用返回下标，所以也就不用存储下标了。

方法一：
```python
class Solution:
    def fourSumCount(self, nums1: List[int], nums2: List[int], nums3: List[int], nums4: List[int]) -> int:
        a1 = self.project(nums1, nums2)
        a3 = self.project(nums3, nums4)
        cnt = 0
        for num in a1:
            if -num in a3:
                cnt += a1[num]*a3[-num]
                
        return cnt

    def project(self, num1: List[int], num2: List[int]) -> dict:
        hashmap = dict()
        for n1 in num1:
            for n2 in num2:
                if n1 + n2 not in hashmap:
                    hashmap[n1+n2] = 1
                else:
                    hashmap[n1+n2] += 1
                
        return hashmap
```
方法二：看了卡哥的代码之后，发现不需要再做一遍这个a3的存储，只需要同样以构造a3的时间复杂度去遍历后两个数组就可以了，并且也不用排列组合了，很方便，甚至省了我方法一最后的一层遍历。
```python
class Solution:
    def fourSumCount(self, nums1: List[int], nums2: List[int], nums3: List[int], nums4: List[int]) -> int:
        a1 = self.project(nums1, nums2)
        
        cnt = 0
        for n3 in nums3:
            for n4 in nums4:
                if -n3-n4 in a1:
                    cnt += a1[-n3-n4]

        return cnt

    def project(self, num1: List[int], num2: List[int]) -> dict:
        hashmap = dict()
        for n1 in num1:
            for n2 in num2:
                if n1 + n2 not in hashmap:
                    hashmap[n1+n2] = 1
                else:
                    hashmap[n1+n2] += 1
                
        return hashmap
```
方法三：其实是第三个submission罢了，这个根本没有改变方法哈，只是对比一下发现**自建函数比较快**，上面比下面快
```python
class Solution:
    def fourSumCount(self, nums1: List[int], nums2: List[int], nums3: List[int], nums4: List[int]) -> int:
        hashmap = dict()
        for n1 in nums1:
            for n2 in nums2:
                if n1 + n2 not in hashmap:
                    hashmap[n1+n2] = 1
                else:
                    hashmap[n1+n2] += 1

        
        cnt = 0
        for n3 in nums3:
            for n4 in nums4:
                if -n3-n4 in hashmap:
                    cnt += hashmap[-n3-n4]

        return cnt
```
方法四：和代码随想录提供的思路学的，熟悉了一下`collections`库里的`defaultdict`函数使用方法，但是挺慢的
```python
from collections import defaultdict
class Solution:
    def fourSumCount(self, nums1: List[int], nums2: List[int], nums3: List[int], nums4: List[int]) -> int:
        
        hashmap = defaultdict(dict)
        for n1 in nums1:
            for n2 in nums2:
                if n1 + n2 not in hashmap:
                    hashmap[n1+n2] = 1
                else:
                    hashmap[n1+n2] += 1

        
        cnt = 0
        for n3 in nums3:
            for n4 in nums4:
                if -n3-n4 in hashmap:
                    cnt += hashmap[-n3-n4]

        return cnt
```
## Leetcode 383 赎金信 
题目链接：[383. 赎金信](https://leetcode.com/problems/ransom-note/description/)

Given two strings `ransomNote` and `magazine`, return `true` if `ransomNote` can be constructed by using the letters from `magazine` and `false` otherwise.

Each letter in `magazine` can only be used once in `ransomNote`.

**思路**：自己的思路
1. 因为是用ransomnote组成magazine，所以呢，把ransomnote做成字典，key当字母，value用来计数；
2. 如果字母出现不止一次，则value计数值++；
3. 然后遍历magazine，返回false的情况：
    - 如果出现了不一样的字母；
    - 字母出现在字典中一次，计数值就-1，当计数值减为负值；
4. 以上都没出现，那就true。

***Tips：*** 这题涉及到字典会比magazine的字母多的情况，因此用这种计数法来判断，而不是单纯的字典最后等于0就可以判断了。

代码如下：
```python
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        hm = dict()

        for cha in magazine:
            if cha not in hm:
                hm[cha] = 1
            else:
                hm[cha] += 1
        
        for cha in ransomNote:
            if cha in hm and hm[cha]>0:
                hm[cha] -= 1
            else:
                return False

        return True
```

## Leetcode 15 三数之和
题目链接：[15. 三数之和](https://leetcode.com/problems/3sum/description/)

Given an integer array nums, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j, i != k, and j != k`, and ` nums[i] + nums[j] + nums[k] == 0`.

Notice that the solution set **must not contain duplicate triplets.**


**思路**： 这题好难啊，我自己想的方法是哈希+双指针遍历，但是过了308/312，最后超时了。没有考虑任何的剪枝。
1. 把数组先存进一个list里面，方便下面要找下标；
2. 双指针，平方遍历，找等于-(a+b)的值在不在list里；
3. 找到所有符合条件的组合后进行去重，满足条件：
    - 三者下标不相等；
    - 不能已经存过了。

这题乍一看和[454. 四数相加II](https://leetcode.com/problems/4sum-ii/description/)很像，但是有以下几个不一样的重点决定了不能用哈希表做：
- 本题是单一数组，四数相加是四个独立的数组；
- 本题有不能重复的双重要求：
    - 下标不得重复；
    - 同样的数不能重复（这个比较难理解，我举个例子：[-1, 1, 2, 0, 1]这个数组里面，[-1, 1, 0]和[-1, 0, 1]满足了下标没有重复，但是排序之后是同一组，因此也要去重）。

就是因为这些原因，导致了如果用哈希表来去重的话，得先遍历找到所有符合条件的情况，你可以讨论所有剪枝使他缩短时间，但是判断条件过于细碎，容易出错。

方法一：哈希表+双指针遍历（自己写的，308/312）
```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        # 输出要求是list
        result = list() 
        # 把原数组存放成list方便底下返回下标
        valuelist = list() 
        for num in nums:
            valuelist.append(num)
        # 双指针找，然后判断-(a+b)是否在valuelist里面
        for i in range(len(nums)):
            for j in range(i+1, len(nums)):
                num = -nums[i]-nums[j]
                if num in valuelist:
                    idx = valuelist.index(num)
                    # 找到所有符合条件的组合然后用if去重 
                    if i != idx and j != idx and sorted([nums[i], nums[j], num]) not in result:
                        result.append(sorted([nums[i], nums[j], num]))
                else:
                    pass
        return result
```
方法二：双指针法

1. 考虑到对下标没有返回的要求，先对数组排序；
2. 从头到尾第一层for循环扫描，此时进行**一级剪枝**：
    - 剪枝一：如果第一个值就大于0，即，最小的值就已经大于0了，那再往后加更不可能等于0，直接`break`或者`return`跳出循环。
    - 剪枝二：如果`nums[i]`的值与前一个`nums[i-1]`的值相等，那就重复计算了，所以continue进行去重。
3. 二级循环双指针开始：
    - 设置左指针从i的后一个位置开始，右指针从尾部开始
    - 类似binary search，如果求和比0大就把right往前挪，小就把left往后挪，相等就把这组数放到result里存起来
    - **二级剪枝**如下：
        - 剪枝一：在left没越过right的情况下，如果right的前面一位和当前位置值相等，那就重复计算了，二级去重right指针。
        - 剪枝二：同理，如果left的后面一位和当前位相等，二级去重left指针。
    - 更新left和right指针。

代码如下：
```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        # no need for the index so we can sort this array
        nums.sort()
        result = []
        for i in range(len(nums)-2):
            if nums[i] > 0:
                # the smallest num > 0 then cannot be able
                return result
            
            # avoid computing same value
            if i > 0 and nums[i] == nums[i-1]:
                continue
            
            left = i + 1    # cursor
            right = len(nums) - 1   # like a sliding window

            while left < right:
                sums = nums[i] + nums[left] + nums[right]
                # sliding window
                if sums > 0:
                    right -= 1
                elif sums < 0:
                    left += 1
                else:
                    result.append([nums[i], nums[left], nums[right]])
                    # avoid the same value computing
                    while right > left and nums[right] == nums[right - 1]:
                        right -= 1
                    while left < right and nums[left] == nums[left + 1]:
                        left += 1
                    
                    right -= 1
                    left += 1

        return result
```

## Leetcode 18 四数之和 
题目链接：[18. 四数之和](https://leetcode.com/problems/two-sum/description/)

Given an array nums of n integers, return an array of all the unique quadruplets `[nums[a], nums[b], nums[c], nums[d]]` such that:

    0 <= a, b, c, d < n
    a, b, c, and d are distinct.
    nums[a] + nums[b] + nums[c] + nums[d] == target
    
You may return the answer in **any order**.

**思路**：[15. 三数之和 ](https://leetcode.com/problems/3sum/description/)的双指针解法是一层`for`循环`num[i]`为确定值，然后循环内有`left`和`right`下标作为双指针，找到`nums[i] + nums[left] + nums[right] == 0`。

四数之和的双指针解法是两层`for`循环`nums[k] + nums[i]`为确定值，依然是循环内有`left`和`right`下标作为双指针，找出`nums[k] + nums[i] + nums[left] + nums[right] == target`的情况，三数之和的时间复杂度是O(n^2)，四数之和的时间复杂度是O(n^3) 。

那么一样的道理，五数之和、六数之和等等都采用这种解法。

对于[15. 三数之和 ](https://leetcode.com/problems/3sum/description/)双指针法就是将原本暴力O(n^3)的解法，降为O(n^2)的解法，四数之和的双指针解法就是将原本暴力O(n^4)的解法，降为O(n^3)的解法。

代码如下：
```python
class Solution:
    def fourSum(self, nums: List[int], target: int) -> List[List[int]]:
        result = []
        nums.sort()
        # 第一层和第二层的去重方法一样
        for i in range(len(nums)):
            if nums[i] > target and nums[i] > 0:
                break
            if i > 0 and nums[i] == nums[i-1]:
                continue

            for j in range(i+1, len(nums)):
                if nums[i]+nums[j] > target and nums[j] > 0:
                    break
                if j > i+1 and nums[j] == nums[j-1]:
                    continue

                left = j + 1
                right = len(nums) - 1 
                while left < right:
                    sums = nums[i] + nums[j] + nums[left] + nums[right]
                    if sums > target:
                        right -= 1
                    elif sums < target:
                        left += 1
                    else:
                        result.append([nums[i], nums[j], nums[left], nums[right]])
                        while left < right and nums[left] == nums[left+1]:
                            left += 1
                        while right > left and nums[right] == nums[right-1]:
                            right -= 1
                        
                        left += 1
                        right -= 1
        return result
```


## 总结

时长：四五六个小时吧，今天超久

知识速览：
`enumerate()`, `break`, `continue`

`dict.values()`, `dict.keys()`, `dict.items()`

`a = list() ==  a = []`

`b = dict() == b = {}`

`from collections import defaultdict`

`defaultdict()`
