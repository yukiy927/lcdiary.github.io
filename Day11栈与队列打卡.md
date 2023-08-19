---
title: Day11栈与队列打卡
date: 2023-08-19 13:40:47
tags: lc打卡
---
### 今日内容： 

● 20. 有效的括号
● 1047. 删除字符串中的所有相邻重复项
● 150. 逆波兰表达式求值


### 重点知识点：

#### Python中整除//和int取整的区别：

//在英文中叫做**floor division**

aka rounds towards floor $-\infty$，也就是向下取整，永远向下取整，因此如果是负数，比如-0.3，就会得到-1。

而int取整，for floating point numbers, this truncates towards zero.也就是向零截断。

So they will diverge for any division in the negatives.负数的时候int只会得到0，和//就很不一样了。

## Leetcode 20. 有效的括号
题目链接：[20. 有效的括号](https://leetcode.cn/problems/valid-parentheses/)

给定一个只包括 `'('，')'，'{'，'}'，'['，']' `的字符串` s `，判断字符串是否有效。

有效字符串需满足：

左括号必须用相同类型的右括号闭合。

左括号必须以正确的顺序闭合。

每个右括号都有一个对应的相同类型的左括号。

**思路**：

1. 首先这个栈，只存左括号；
2. 其次，如果要存的是右括号，此时栈空，false
3. 如果栈顶就是对应的左括号，弹出栈顶

```python
class Solution:
    def isValid(self, s: str) -> bool:
        """长度不成对，肯定不成立"""
        if len(s) % 2:
            return False
        
        """字典用来只存左括号"""
        hm = {'{':'}', '[':']','(':')'}

        """利用栈来实现"""
        stack = []
        for i in range(len(s)):
            """
            首先这个栈，只存左括号；
            其次，如果要存的是右括号，此时栈空，false
            如果栈顶就是对应的左括号，弹出栈顶
            """
            if s[i] in hm:
                stack.append(s[i])
            else:
                if stack == []:
                    return False
                elif s[i] == hm[stack[-1]]:
                    stack.pop()
                else:       # 考虑这种情况([{{])
                    return False
            
        """最后栈里必须为空"""
        if len(stack) != 0:
            return False
        else:
            return True     
```


## Leetcode 1047. 删除字符串中的所有相邻重复项
题目链接：[1047. 删除字符串中的所有相邻重复项](https://leetcode.cn/problems/remove-all-adjacent-duplicates-in-string/)

给出由小写字母组成的字符串` S `，重复项删除操作会选择两个相邻且相同的字母，并删除它们。

在` S `上反复执行重复项删除操作，直到无法继续删除。

在完成所有重复项删除操作后返回最终的字符串。答案保证唯一。

<!-- ![swap_ex1](https://github.com/yukiy927/lcdiary.github.io/blob/main/pics/swap_ex1.jpg) -->

**思路**：甚至比上面一题还简单，一样的用栈做消消乐。


```python
class Solution:
    def removeDuplicates(self, s: str) -> str:
        stack = []
        for char in s:
            """为空就存"""
            if stack == []:
                stack.append(char)
            else:
                # 如果和栈顶相等，就pop
                if char == stack[-1]:
                    stack.pop()
                # 不相等就存进来
                else:
                    stack.append(char)
        """转成字符串输出"""    
        return ''.join(stack)
```

## Leetcode 150. 逆波兰表达式求值
题目链接：[150. 逆波兰表达式求值](https://leetcode.cn/problems/evaluate-reverse-polish-notation/)

给你一个字符串数组` tokens `，表示一个根据 逆波兰表示法 表示的算术表达式。

请你计算该表达式。返回一个表示表达式值的整数。

注意：

- 有效的算符为 '+'、'-'、'*' 和 '/' 。

- 每个操作数（运算对象）都可以是一个整数或者另一个表达式。

- 两个整数之间的除法总是 向零截断 。

- 表达式中不含除零运算。

- 输入是一个根据逆波兰表示法表示的算术表达式。

- 答案及所有中间计算结果可以用 32 位 整数表示。

<!-- ![swap_ex1](https://github.com/yukiy927/lcdiary.github.io/blob/main/pics/swap_ex1.jpg) -->
**逆波兰表达式：**

逆波兰表达式是一种后缀表达式，所谓后缀就是指算符写在后面。

- 平常使用的算式则是一种中缀表达式，如 ( 1 + 2 ) * ( 3 + 4 ) 。
- 该算式的逆波兰表达式写法为 ( ( 1 2 + ) ( 3 4 + ) * ) 。

逆波兰表达式主要有以下两个优点：

- 去掉括号后表达式无歧义，上式即便写成 1 2 + 3 4 + * 也可以依据次序计算出正确结果。
- 适合用栈操作运算：遇到数字则入栈；遇到算符则取出栈顶两个数字进行计算，并将结果压入栈中

**思路**：适合用栈操作运算：遇到数字则入栈；遇到算符则取出栈顶两个数字进行计算，并将结果压入栈中。

注意floor division // 和int取整的区别。

方法一：
```python
class Solution:
    def evalRPN(self, tokens: List[str]) -> int:
        op = ['+', '-', '*', '/']
        stack = []
        for i in tokens:
            if i not in op:
                stack.append(int(i))
            else:
                num2 = stack.pop()
                num1 = stack.pop()
                if i == op[0]:
                    stack.append(num1+num2)
                elif i == op[1]:
                    stack.append(num1-num2)
                elif i == op[2]:
                    stack.append(num1*num2)
                else:
                    stack.append(int(num1/num2))
                # match i:
                #     case '+':
                #         stack.append(num1+num2)
                #     case '-':
                #         stack.append(num1-num2)
                #     case '*':
                #         stack.append(num1*num2)
                #     case '/':
                #         stack.append(num1//num2)

        return stack.pop()
```
方法二（熟悉一下这种函数的用法）：
```python
from operator import add, sub, mul

class Solution:
    op_map = {'+': add, '-': sub, '*': mul, '/': lambda x, y: int(x / y)}
    
    def evalRPN(self, tokens: List[str]) -> int:
        stack = []
        for token in tokens:
            if token not in {'+', '-', '*', '/'}:
                stack.append(int(token))
            else:
                op2 = stack.pop()
                op1 = stack.pop()
                stack.append(self.op_map[token](op1, op2))  # 第一个出来的在运算符后面
        return stack.pop()
```

### 总结

今天晚上美团笔试，五道编程。好难呀，感觉对acm输入输出模式非常不熟悉，要加速刷题了。

AC率：150/500
1. 叫号
2. 魔法乘，算法优化
3. 01串，权值计算，全部子串权值和
4. ab双数组，同位不重复，和相等，正整数
5. 众数，一个加一，一个减一