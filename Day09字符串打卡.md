---
title: Day09字符串打卡
date: 2023-08-17 08:13:44
tags: lc打卡
---
● 28. 实现 strStr()
● 459. 重复的子字符串
● 字符串总结 
● 双指针回顾 

看了KMP，没看懂，拉倒吧
## Leetcode 28. 实现 strStr() 
题目链接：[28. 实现 strStr()](https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/description/)

Given two strings needle and haystack, return the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.

<!-- ![swap_ex1](https://github.com/yukiy927/lcdiary.github.io/blob/main/pics/swap_ex1.jpg) -->

**思路**：



```python
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        n = len(needle)
        for j in range(len(haystack)):
            # 利用切片，O(n)
            if haystack[j:j+n] == needle:
                return j
        return -1
```

## Leetcode 459.重复的子字符串
题目链接：[459.重复的子字符串](https://leetcode.com/problems/repeated-substring-pattern/description/)

Given a string s, check if it can be constructed by taking a substring of it and appending multiple copies of the substring together.

**思路**：

```python
class Solution:
    def repeatedSubstringPattern(self, s: str) -> bool:
        n = len(s)
        if n <= 1:
            return False
        
        substr = ''
        # 最少的重复次数是两次
        for i in range(1, n//2 + 1):
            # 只有当长度能整除才进入
            if n % i == 0:
                substr = s[:i]  # 切片
                if substr * (n//i) == s:    # 如果能重复
                    return True

        return False
```

## Leetcode 字符串总结


## Leetcode 双指针回顾


## 总结

今天把简历又改了一遍，国内+国外，以及投简历，房子合约也签了，Verizon电话也打了，好累哦最近，琐碎的事情一堆一堆的。