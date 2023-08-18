---
title: Day10栈与队列打卡
date: 2023-08-18 05:44:48
tags: lc打卡
---
今日任务：

● 理论基础
● 232. 用栈实现队列
● 225. 用队列实现栈

今日知识速递：

1. 在Python中，List本质上是一个长度可变的连续数组。

2. 针对昨日字符串加以一些补充小知识，Python的字符串具有不可变的性质，因此当题目要求in-place的更改的时候，如果我们使用到切片

```python
# 这里会报错，因为=是一个assign的符号，s[::-1]是新开的一个副本，占用了新的内存地址，这个是赋值给s会更改s的内存地址，所以就不是原地更改了。
s = s[::-1]         
# 这里是相当于把s的元素更新赋值了，因此是属于in-place的。
s[:] = s[::-1]
```

3. [栈](https://www.bilibili.com/video/BV1qV411z7FN?p=2&vd_source=505eec5b99c590cfd236cb4cb97733f3) -- 后进先出 LIFO

```python
class Stack(object):
    """栈"""
    def __init__(self):
        self.__list = []
    
    def push(self, item):
        """添加一个新的元素item到栈顶"""
        self.__list.append(item)

    def pop(self):
        """弹出栈顶元素"""
        return self.__list.pop()

     def peek(self):
        """返回栈顶元素"""
        if self.is_empty():
            return None
        return self.__list[-1]

    def is_empty(self):
        """判断栈是否为空，空就返回True"""
        return not self.__list

    def size(Self):
        """返回栈的元素个数"""
        return len(self.__list)

    if __name__ == "__main__":
        s = Stack()
        s.push(1)
        s.push(2)
        s.push(3)
        print(s.pop())
        print(s.pop())
        print(s.pop())
```

4. [队列](https://www.bilibili.com/video/BV1qV411z7FN?p=2&vd_source=505eec5b99c590cfd236cb4cb97733f3) -- 先进先出 FIFO

队列的实现：
```python
class Queue(object):
    """队列"""
    def __init__(self):
        self.__list = []
    
    def enqueue(self, item):
        """往队列中添加一个item元素"""
        self.__list.append(item)        # 尾进，头出
        # self.__list.insert(0, item)   # 头进，尾出，时间复杂度为O(n)

    def dequeue(self):
        """从队列头部删除一个元素"""
        return self.__list.pop(0)   # 时间复杂度为O(n)
        # return self.__list.pop()

        """
        怎么选择就看是添加的操作需求大还是删除的操作更频繁。
        """

    def is_empty(self):
        """判断队列是否为空，空就返回True"""
        return self.__list == []

    def size(self):
        """返回队列的元素个数"""
        return len(self.__list)

    if __name__ == "__main__":
        s = Stack()
        s.enqueue(1)
        s.enqueue(2)
        s.enqueue(3)
        print(s.dequeue())
        print(s.dequeue())
        print(s.dequeue())    
```
双端队列的实现：
```python
class Deque(object):
    """双端队列"""
    def __init__(self):
        self.__list = []
    
    def add_front(self, item):
        """往队列头部添加一个item元素"""
        self.__list.insert(0, item)

    def add_rear(self, item):
        """往队列尾部添加一个item元素"""
        self.__list.append(item)

    def pop_front(self):
        """从队列头部删除一个元素"""
        return self.__list.pop(0)
    
    def pop_rear(self):
        """从队列尾部删除一个元素"""
        return self.__list.pop()

    def is_empty(self):
        """判断队列是否为空，空就返回True"""
        return self.__list == []

    def size(self):
        """返回队列的元素个数"""
        return len(self.__list)

    if __name__ == "__main__":
        s = Stack()
        # s.enqueue(1)
        # s.enqueue(2)
        # s.enqueue(3)
        # print(s.dequeue())
        # print(s.dequeue())
        # print(s.dequeue())    
```

5. 二者都可以用数组（list）和链表实现，还有双向队列（deque）

## Leetcode 232. 用栈实现队列
题目链接：[232. 用栈实现队列](https://leetcode.com/problems/implement-queue-using-stacks/)

Implement a first in first out (FIFO) queue using **only two stacks**. The implemented queue should support all the functions of a normal queue (`push`, `peek`, `pop`, and `empty`).

Implement the `MyQueue` class:

`void push(int x)` Pushes element x to the back of the queue.

`int pop() `Removes the element from the front of the queue and returns it.

`int peek() `Returns the element at the front of the queue.

`boolean empty() `Returns true if the queue is empty, false otherwise.

Notes:

You must use only standard operations of a stack, which means only push to top, peek/pop from top, size, and is empty operations are valid.

Depending on your language, the stack may not be supported natively. You may simulate a stack using a `list` or `deque` (double-ended queue) as long as you use only a stack's standard operations.

**思路**：两道都是设计题，真想画个图。

1. 两个栈，用list实现。
2. 栈in用来存，栈out用来出。
3. 当需要pop的时候，栈in的元素全部pop到栈out中，在栈out中，就变成逆序了，此时pop栈顶刚好是第一个存入的元素，实现了队列。
```python
class MyQueue:
    
    def __init__(self):
        '''
        in用来push，out用来pop
        '''
        self.stack_in = []
        self.stack_out = []

    def push(self, x: int) -> None:
        '''
        有新的元素进来，就存在in里
        '''
        self.stack_in.append(x)

    def pop(self) -> int:
        '''
        如果空，就返回none
        要是out空，就把in里的pop出来push到out里
        要是out不空，就pop她
        '''
        if self.empty():
            return None

        if self.stack_out:
            return self.stack_out.pop()
        else:
            for i in range(len(self.stack_in)):
                self.stack_out.append(self.stack_in.pop())
            return self.stack_out.pop()

    def peek(self) -> int:
        '''
        直接pop，然后再存到out里，因为是顶嘛，最先存进来的
        '''
        ans = self.pop()
        self.stack_out.append(ans)
        return ans

    def empty(self) -> bool:
        '''
        两个都空才叫空;
        如果队列为空，返回 true ；否则，返回 false
        '''
        return not (self.stack_in or self.stack_out)


# Your MyQueue object will be instantiated and called as such:
# obj = MyQueue()
# obj.push(x)
# param_2 = obj.pop()
# param_3 = obj.peek()
# param_4 = obj.empty()
```

## Leetcode 225. 用队列实现栈
题目链接：[225. 用队列实现栈](https://leetcode.com/problems/implement-stack-using-queues/)

Implement a last-in-first-out (LIFO) stack using **only two queues**. The implemented stack should support all the functions of a normal stack (`push`, `top`, `pop`, and `empty`).

Implement the `MyStack` class:

`void push(int x) `Pushes element x to the top of the stack.

`int pop() `Removes the element on the top of the stack and returns it.

`int top()` Returns the element on the top of the stack.

`boolean empty() `Returns true if the stack is empty, false otherwise.

Notes:

You must use only standard operations of a queue, which means that only push to back, peek/pop from front, size and is empty operations are valid.

Depending on your language, the queue may not be supported natively. You may simulate a queue using a `list` or `deque` (double-ended queue) as long as you use only a queue's standard operations.


<!-- ![swap_ex1](https://github.com/yukiy927/lcdiary.github.io/blob/main/pics/swap_ex1.jpg) -->

**思路**：



方法一（永远是自己写的，思路可以看，可是建议用方法二）：
1. 两个队列，用list实现
2. 在pop的时候两个来回倒，倒到最后一个元素留在原队列中时，弹出
3. 这个时候该队列就空了，如果再pop就重复该操作，但对象是第二个队列
4. 用if判断哪个队列为空，就弹另一个
```python
class MyStack:

    def __init__(self):
        self.queue1 = []
        self.queue2 = []

    def push(self, x: int) -> None:
        if self.queue1 == []:
            self.queue2.append(x)
        elif self.queue2 == []:
            self.queue1.append(x)

    def pop(self) -> int:
        if self.empty():
            return None

        if self.queue1 and not self.queue2:
            while len(self.queue1) > 1:
                self.queue2.append(self.queue1.pop(0))
            return self.queue1.pop()
        elif self.queue2 and not self.queue1:
            while len(self.queue2) > 1:
                self.queue1.append(self.queue2.pop(0))
            return self.queue2.pop()

    def top(self) -> int:
        ans = self.pop()
        self.push(ans)
        return ans

    def empty(self) -> bool:
        return not (self.queue1 or self.queue2)


# Your MyStack object will be instantiated and called as such:
# obj = MyStack()
# obj.push(x)
# param_2 = obj.pop()
# param_3 = obj.top()
# param_4 = obj.empty()
```
方法二：

1. 依然是两个队列，list实现
2. 但是此时在pop那里每次弹完就交换回来，令que1永远是存数据的，que2只有在pop函数实现的时候做tmp list交换使用
3. 其余都和方法一一样

```python
class MyStack:

    def __init__(self):
        # 只用1存，2在pop的时候用来交换
        self.queue1 = []
        self.queue2 = []

    def push(self, x: int) -> None:
        
        self.queue1.append(x)

    def pop(self) -> int:
        if self.empty():
            return None

        while len(self.queue1) > 1:
            self.queue2.append(self.queue1.pop(0))
        ans = self.queue1.pop()
        # 交换
        self.queue1, self.queue2 = self.queue2, self.queue1

        return ans

    def top(self) -> int:
        ans = self.pop()
        self.push(ans)
        return ans

    def empty(self) -> bool:
        return not (self.queue1 or self.queue2)


# Your MyStack object will be instantiated and called as such:
# obj = MyStack()
# obj.push(x)
# param_2 = obj.pop()
# param_3 = obj.top()
# param_4 = obj.empty()
```

方法三（利用deque双向队列）：

也是个非常不错的办法，找到模拟的规律

1. 其实难点就在pop，双向队列我们只用append和popleft，因为list的pop(0)时间复杂度是O(n)
2. 实现pop函数的时候，左弹popleft元素然后append到队列的尾部，直到最后存进的元素在队列的第一个，然后左弹，完美

```python
class MyStack:
    '''
    一个队列在模拟栈弹出元素的时候只要将队列头部的元素（除了最后一个元素外） 重新添加到队列尾部，此时再去弹出元素就是栈的顺序了。
    '''
    def __init__(self):
        self.queue = deque()

    def push(self, x: int) -> None:
        self.queue.append(x)

    def pop(self) -> int:
        if self.empty():
            return None
        for i in range(len(self.queue) - 1):
            self.queue.append(self.queue.popleft())
        return self.queue.popleft()

    def top(self) -> int:
        ans = self.pop()
        self.push(ans)
        return ans
        '''
        以下代码是替换代码，但是没上面的速度快
        '''
        # if self.empty():
        #     return None
        # return self.queue[-1]

    def empty(self) -> bool:
        return not self.queue


# Your MyStack object will be instantiated and called as such:
# obj = MyStack()
# obj.push(x)
# param_2 = obj.pop()
# param_3 = obj.top()
# param_4 = obj.empty()
```
## Carl哥说：


可以看出peek()的实现，直接复用了pop()， 要不然，对stOut判空的逻辑又要重写一遍。

再多说一些代码开发上的习惯问题，在工业级别代码开发中，最忌讳的就是 实现一个类似的函数，直接把代码粘过来改一改就完事了。

这样的项目代码会越来越乱，一定要懂得复用，功能相近的函数要抽象出来，不要大量的复制粘贴，很容易出问题！（踩过坑的人自然懂）

工作中如果发现某一个功能自己要经常用，同事们可能也会用到，自己就花点时间把这个功能抽象成一个好用的函数或者工具类，不仅自己方便，也方便了同事们。

同事们就会逐渐认可你的工作态度和工作能力，自己的口碑都是这么一点一点积累起来的！在同事圈里口碑起来了之后，你就发现自己走上了一个正循环，以后的升职加薪才少不了你！哈哈哈

## 总结

今天把昨天没理解的kmp算法看了，视频链接贴在这了[最浅显易懂的KMP算法讲解](https://www.bilibili.com/video/BV1AY4y157yL/?spm_id_from=333.337.search-card.all.click&vd_source=505eec5b99c590cfd236cb4cb97733f3)把数据结构一点一点啃了之后，发现实现起来没那么难，收到了两家拒信两家oa，准备做做历届笔试题，以及多投简历，继续在b站上把二叉树[Python数据结构与算法06——树与树算法](https://www.bilibili.com/video/BV1NZ4y1M7wo/?spm_id_from=333.880.my_history.page.click&vd_source=505eec5b99c590cfd236cb4cb97733f3)+贪心给看了。