---
title: Day08字符串打卡
date: 2023-08-16 02:02:32
tags: lc打卡
---
## 字符串知识点
Python中，***字符串本身不可以被更改***，所以如果需要操作字符串，得先转换成list或者存在数组中。如果返回值需要字符串，可以利用`join()`函数整合后再返回。
### I.  `[:: -1]`的理解（同样遵循左闭右开）：
1. 适用对象：字符串、列表、数组。

2. 作用：反转

3. 参数：
    - 第一个colon` ： `是指反转对象的起始下标，这里指默认是最开始
    - 第二个colon` ： `是指反转对象的结束下标，这里指默认是最结尾
    - 第三个`-1`：是指步长，相当于从后往前输出，下标一直做减一

4. 举例： `[5:0:-1]`    
```python
>>>nums = [0, 1, 2, 3, 4, 5, 6]
>>>print(nums[5:0:-1])
[5, 4, 3, 2, 1]
```
### II. swap库函数的两种实现方法：

一种就是常见的交换数值：
```c++
int tmp = s[i];
s[i] = s[j];
s[j] = tmp;
```
一种就是通过位运算：
```c++
s[i] ^= s[j];
s[j] ^= s[i];
s[i] ^= s[j];
```
**这里的位运算怎么理解？**
### III. Python字符串的一些自建函数
#### 1. split
Python `split() `通过指定分隔符对字符串进行切片，如果参数` num `有指定值，则分隔` num+1 `个子字符串。
```python
str.split(str="", num=string.count(str)).
```
str -- 分隔符，默认为所有的空字符：空格、换行\n、制表符\t等。

num -- 分割次数，默认为-1，即分割所有。
```python
#!/usr/bin/python
# -*- coding: UTF-8 -*-
 
str = "Line1-abcdef \nLine2-abc \nLine4-abcd";
print str.split( );       # 以空格为分隔符，包含 \n
print str.split(' ', 1 ); # 以空格为分隔符，分隔成两个
```
以上实例输出结果如下：
```python
['Line1-abcdef', 'Line2-abc', 'Line4-abcd']
['Line1-abcdef', '\nLine2-abc \nLine4-abcd']
```
#### 2. strip
Python `strip() `方法用于移除字符串头尾指定的字符（默认为空格或换行符）或字符序列。

注意：该方法只能删除开头或是结尾的字符，不能删除中间部分的字符。
```python
str.strip([chars])  # 这里默认是头尾的空格或换行符
```
```python
#!/usr/bin/python
# -*- coding: UTF-8 -*-
 
str = "00000003210Runoob01230000000"
print str.strip( '0' )  # 去除首尾字符 0
 
 
str2 = "   Runoob      "  # 去除首尾空格
print str2.strip()
```
以上实例输出结果如下：
```python
3210Runoob0123
Runoob
```
## Leetcode 344.反转字符串 
题目链接：[344.反转字符串](https://leetcode.com/problems/reverse-string/)

Write a function that reverses a string. The input string is given as an array of characters `s`.

You must do this by modifying the input array **in-place** with `O(1)` extra memory.

<!-- ![swap_ex1](https://github.com/yukiy927/lcdiary.github.io/blob/main/pics/swap_ex1.jpg) -->

**思路**：题目中把字符串存在了数组里，这样就不存在python中字符串不可改性了。
- 双指针left和right，一个从头走，另一个从尾走
- 同时移动，然后交换彼此位置


```python
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        left = 0
        right = len(s) - 1
        while left < right:
            s[left], s[right] = s[right], s[left]
            left += 1
            right -= 1
        
```

## Leetcode 541. 反转字符串II
题目链接：[541. 反转字符串II](https://leetcode.com/problems/reverse-string-ii/)

Given a string `s `and an integer `k`, reverse the first `k` characters for every `2k` characters counting from the start of the string.

If there are fewer than `k` characters left, reverse all of them. If there are less than `2k` but greater than or equal to `k` characters, then reverse the first `k` characters and leave the other as original.

**思路**：题目对于我来说，难点在于下标的更新和边界。

- 令起始指针永远指向`2k`长度子串的开头位置
- 然后利用**切片**反转
- 再更新起始指针`cur`

以下两个方法核心思想都和上面写的差不多，知识可能会涉及到不同的库函数的应用以及切片。

方法一（利用344创建的`reverse()`函数）：
```python
class Solution:
    def reverseStr(self, s: str, k: int) -> str:
        '''
        结合344反转字符串，定义函数reverse()
        '''
        def reverse(text) -> str:
            left = 0
            right = len(text) - 1
            while left < right:
                text[left], text[right] = text[right], text[left]
                left += 1
                right -= 1
            return text

        res = list(s)   # 函数reverse只能对于list操作

        for cur in range(0, len(s), 2*k):
            res[cur:cur+k] = reverse(res[cur:cur+k])
        
        return "".join(res)
```
方法二（利用切片直接进行反转，省去`reverse`）：
```python
class Solution:
    def reverseStr(self, s: str, k: int) -> str:
        p = 0

        while p < len(s):
            p2 = p + k
            # p是起始位置，p到p2进行反转
            s = s[:p] + s[p:p2][::-1] + s[p2:]
            # 当最后一个完整的2k反转完之后，此时p加到剩余字符的起始位置
            p += 2*k

        return s
```

## Leetcode 剑指Offer 05.替换空格
题目链接：[剑指Offer 05.替换空格](https://leetcode.cn/problems/ti-huan-kong-ge-lcof/description/)

请实现一个函数，把字符串 s 中的每个空格替换成"%20"。


方法一（先创后存，有点水题的感觉，因为python语言本身的问题）：
```python
class Solution:
    def replaceSpace(self, s: str) -> str:
        # py中字符串是不可变型，所以操作字符串需要转换成列表，因此空间复杂度不可能是O(1)
        res = []
        # 先存到列表里，然后利用join合起来
        for i in range(len(s)):
            if s[i] == " ":
                res.append("%20")
            else:
                res.append(s[i])

        return "".join(res)
        
```
方法二（双指针法）：
- 先多出空格
- 从后往前重新覆盖元素

```python
class Solution:
    def replaceSpace(self, s: str) -> str:
        # py中字符串是不可变型，所以操作字符串需要转换成列表，因此空间复杂度不可能是O(1)
        counter = s.count(' ')  # 计算空格的数量
        
        res = list(s)
        res.extend([' ']*counter*2) # 三替一，多出每两个位置

        left, right = len(s) - 1, len(res) - 1

        while left >= 0:
            # 从后往前，如果遇到非空格元素，就复制
            if res[left] != ' ':
                res[right] = res[left]
            # 如果遇到空格，就把三个位置更新成新元素，指针前移2个
            else:
                res[right-2:right+1] = '%20'
                right -= 2
            left -= 1
            right -= 1
        
        return ''.join(res)
```

## Leetcode 151.翻转字符串里的单词
题目链接：[151.翻转字符串里的单词](https://leetcode.com/problems/reverse-words-in-a-string/description/)

Given an input string `s`, reverse the order of the **words**.

A **word** is defined as a sequence of non-space characters. The **words** in `s` will be separated by at least one space.

Return *a string of the words in reverse order concatenated by a single space*.

**Note** that `s` may contain leading or trailing spaces or multiple spaces between two words. The returned string should only have a single space separating the words. Do not include any extra spaces.

**思路**：我的思路是，利用双指针left和right，从尾部开始移动，right锁定单词尾部，left去找单词头部，然后利用切片存到新的列表res里面，除了第一个单词以外，后面所有的单词存储都需要在前面加上一个空格。

但是后来发现其实如果借用split函数，完全就是我这道题的思路，但就变成水题了TT，不知道我这算什么呜呜ww/

我的方法一：
```python
class Solution:
    def reverseWords(self, s: str) -> str:
        """
        利用双指针，从后往前存单词到新的列表里。
        第一个存的单词只存单词，
        后面开始就是空格+单词，这样就可以保证结尾没有空格。
        """
        res = []
        right = len(s) - 1
        left = right
        while right >= 0 and left >= 0:
            # 左指针每次跟随右指针，从右指针开始往前移
            left = right
            # 如果遇到空格就跳过
            if s[right] == ' ':
                right -= 1
                continue
            else:
                # left首先不能出界，直到找到单词的第一个字母。
                while s[left - 1] != ' ' and left > 0:
                    # 在这个基础上不停左移
                    left -= 1
                # 如果是第一个单词，那就只存单词
                if res == []:
                    res.append(s[left : right + 1])
                # 后面就开始空格+单词的组合
                else:
                    res.append(' '+s[left : right + 1])
                # 更新右指针到左指针的前面，因为左指针已经存过了
                right = left - 1
        #最后返回整合的字符串
        return ''.join(res)
```
方法二（双指针 + split）：
```python
class Solution:
    def reverseWords(self, s: str) -> str:
        """
        同样是双指针，但是借用了split函数把：
        1.空格问题
        2.单词提取问题
        解决了
        """
        words = s.split()
        
        left, right = 0, len(words) - 1

        while left < right:
            words[left], words[right] = words[right], words[left]
            left += 1
            right -= 1
        
        return ' '.join(words)
```
方法三（strip + 切片）：
```python
class Solution:
    def reverseWords(self, s: str) -> str:
        """
        1.删除空格
        2.反转整个列表
        3.反转每个单词
        """
        # 删除前后空白
        s = s.strip()
        # 反转整个字符串
        s = s[::-1]
        # 将字符串拆分成单词然后反转每个单词
        s = ' '.join(word[::-1] for word in s.split())
        return s
```

## Leetcode 剑指Offer58-II.左旋转字符串
题目链接：[剑指Offer58-II.左旋转字符串](https://leetcode.cn/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/description/)

字符串的左旋转操作是把字符串前面的若干个字符转移到字符串的尾部。请定义一个函数实现字符串左旋转操作的功能。比如，输入字符串"abcdefg"和数字2，该函数将返回左旋转两位得到的结果"cdefgab"。

**思路**：这题Python写我管他叫小题大做

![左旋-反转三次](https://github.com/yukiy927/lcdiary.github.io/blob/main/pics/%E5%89%91%E6%8C%87Offer58-II.%E5%B7%A6%E6%97%8B%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2.png)

方法一（利用切片，一行代码）：
```python
class Solution:
    def reverseLeftWords(self, s: str, n: int) -> str:

        return s[n:]+s[:n]
```
方法二（小题大做，利用反转）：
```python
class Solution:
    def reverseLeftWords(self, s: str, n: int) -> str:
        s = list(s)
        # 前反转
        s[0:n] = list(reversed(s[0:n]))
        # 后反转
        s[n:] = list(reversed(s[n:]))
        # 整体反转
        s.reverse()
        return ''.join(s)
```


## 总结

今天基本都能做出来，但是每次到那种等间隔计数的时候，就会很绕。