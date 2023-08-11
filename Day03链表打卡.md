---
title: Day03链表打卡
date: 2023-08-11 08:40:25
tags: lc打卡
---
## Leetcode 203 移除链表元素

题目链接：[203. 移除链表元素](https://leetcode.com/problems/remove-linked-list-elements/)

Given the head of a linked list and an integer val, remove all the nodes of the linked list that has Node.val == val, and return the new head.

**思路**：熟悉dummyhead这个好东西，虚拟节点可以帮助你解决很多superposition，比如要删的是头节点啊，这个时候头节点前面不就没有一个衔接的东西了吗，虚拟节点就起到效果了，p.s.昨天的排版太丑了，今天换一下。

Tips：试了三种提交，可以发现我最终代码保留了一个head是否空的判断，以及是否删的是尾节点，速度快的不是一点，用条件判断省掉的时间真的很宝贵呀！

代码如下：

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeElements(self, head: Optional[ListNode], val: int) -> Optional[ListNode]:
        dummy_head = ListNode(next=head)
        cur = dummy_head

        if head is None:
            return head
        
        while cur.next:
            if cur.next.val == val and cur.next.next is None:
                cur.next = None
            elif cur.next.val == val and cur.next.next is not None:
                cur.next = cur.next.next
            else:
                cur = cur.next
            
        return dummy_head.next
```

## Leetcode 707 设计链表
题目链接：[707. 设计链表](https://leetcode.cn/problems/minimum-size-subarray-sum/)

Design your implementation of the linked list. You can choose to use a singly or doubly linked list.
A node in a singly linked list should have two attributes: `val` and `next`. `val` is the value of the current node, and `next` is a pointer/reference to the next node.
If you want to use the doubly linked list, you will need one more attribute `prev` to indicate the previous node in the linked list. Assume all nodes in the linked list are 0-indexed.

Implement the `MyLinkedList` class:

`MyLinkedList()`Initializes the `MyLinkedList` object.

`int get(int index)` Get the value of the indexth node in the linked list. If the index is invalid, return -1.

`void addAtHead(int val)` Add a node of value val before the first element of the linked list. After the insertion, the new node will be the first node of the linked list.

`void addAtTail(int val)` Append a node of value val as the last element of the linked list.

`void addAtIndex(int index, int val)` Add a node of value val before the indexth node in the linked list. If index equals the length of the linked list, the node will be appended to the end of the linked list. If index is greater than the length, the node will not be inserted.

`void deleteAtIndex(int index)` Delete the indexth node in the linked list, if the index is valid.

**思路**：只会写单链表，因为之前看b站留了个坑，还没学双链表，呜呜，好烦呦TT

    首先，这道题的要点之一是虚拟节点的创建！！！很重要哦

    其次，我是看了参考代码才知道可以创个`self.size` 非常方便，解决了很多判断问题，判断很容易出错的

    最后，这题其实就是比较缠人，耗时，需要足够仔细，但是本身逻辑性不强，温故而知新吧

代码如下：
```python
class ListNode:
    def __init__(self, val = 0, next = None):
        self.val = val
        self.next = next

class MyLinkedList:

    def __init__(self):
        self.dummyhead = ListNode() # 虚拟节点
        self.size = 0

    def get(self, index: int) -> int:
        cur = self.dummyhead.next
        if index >= self.size or index < 0:
            return -1
        for i in range(index):
            cur = cur.next
        return cur.val


    def addAtHead(self, val: int) -> None:
        head = ListNode(val=val,next=self.dummyhead.next)
        self.dummyhead.next = head
        self.size += 1
        return self.dummyhead.next

    def addAtTail(self, val: int) -> None:
        tail = ListNode(val=val)
        cur = self.dummyhead
        while cur.next:
            cur = cur.next
        cur.next = tail
        self.size += 1
        return self.dummyhead.next

    def addAtIndex(self, index: int, val: int) -> None:
        node = ListNode(val = val)
        cur = self.dummyhead
        if index > self.size or index < 0:
            return

        for i in range(index):
            cur = cur.next
        if index == self.size:
            cur.next = node
        else:
            node.next = cur.next
            cur.next = node
        self.size += 1
        return cur
        

    def deleteAtIndex(self, index: int) -> None:
        cur = self.dummyhead
        if index >= self.size:
            return 
        for i in range(index):
            cur = cur.next
        if cur.next.next is None:
            cur.next = None
        else:
            cur.next = cur.next.next
        self.size -= 1


# Your MyLinkedList object will be instantiated and called as such:
# obj = MyLinkedList()
# param_1 = obj.get(index)
# obj.addAtHead(val)
# obj.addAtTail(val)
# obj.addAtIndex(index,val)
# obj.deleteAtIndex(index)
```

## Leetcode 206 反转链表
题目链接：[206. 反转链表](https://leetcode.com/problems/reverse-linked-list/)

Given the head of a singly linked list, reverse the list, and return the reversed list.

**思路**：重点是一个交换的思想，我觉得有点像那个最基础的交换a和b的值，就是找个temp去存一下，而这里要存的则是链表最难存的——即将失去头的结点（我这名取的好精辟）。剩下的就是更新迭代啦，不过这道题在comment里有个未解之谜，为啥我单独考虑极端情况，就超出内存限制了呢，下次博客争取加点图进来，要不然好单调哦。

解决了的话我就更新在这里：

###########
###########

代码如下：
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        # if head is None or head.next is None:
        #     return head
        
        # pre, cur = head, head.next
        pre, cur = None, head

        while cur:
            tmp = cur.next
            cur.next = pre
            pre = cur
            cur = tmp
        
        return pre

```

（自从day02的时候carl哥让我们开始写总结，每天第一个写的就是总结哈哈哈哈，天知道我有多爱总结^^）

今天又拖到晚上了…………我明天一定改！

发现了carl哥搞的这个进度很有道理，第一天稍微简单点，第二天上难度，今天链表还好，得益于之前有好好看b站打基础，记得不少，题目基本都是自己写出来的，表扬一下今天自己静下心来debug了，并且一顺到底，运行直接AC，蛤蛤，果然集中训练就是手感会好起来，继续保持吧姑娘~

时长：四个小时