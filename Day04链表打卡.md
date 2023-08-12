---
title: Day04链表打卡
date: 2023-08-12 04:21🕥
tags: lc打卡
---
## Leetcode 24 两两交换链表中的节点

题目链接：[24. 两两交换链表中的节点](https://leetcode.com/problems/swap-nodes-in-pairs/)

Given a linked list, swap every two adjacent nodes and return its head. You must solve the problem without modifying the values in the list's nodes (i.e., only nodes themselves may be changed.)

![swap_ex1](https://github.com/yukiy927/lcdiary.github.io/blob/main/pics/swap_ex1.jpg)

**思路**：二刷这道题了，印象很深，所以自己写出来了，**以两个节点为一组**，**组前节点**到组内节点之间更改指向顺序，组内节点与组后节点更改指向顺序。这题对我来说关键是，while的判断条件。

**Tips**：
1. **画图**，这题画图很直观，对着图写代码。
2. while这个判断条件也很好记，因为我们以组为单位两两考虑，所以就得保证剩下的节点是大于等于两个的，如果只剩下单个节点，由于组队不成立，也就不用交换了。
3. 由于与组前节点有关，我们需要引入虚拟节点。好像前面又listnode的def，都要加虚拟节点，待考证。

代码如下：

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def swapPairs(self, head: Optional[ListNode]) -> Optional[ListNode]:
        dummy_head = ListNode(next = head)
        cur = dummy_head
        
        if head is None or head.next is None:
            return head

        while cur.next and cur.next.next:   
            # 当后两个都不为空的时候就循环
            # 剩一个的话也不用交换了
            tmp1 = cur.next
            cur.next = cur.next.next
            tmp1.next = cur.next.next
            cur.next.next = tmp1
            cur = tmp1
            

        return dummy_head.next

```

## Leetcode 19 删除链表的倒数第N个节点
题目链接：[19.删除链表的倒数第N个节点](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)

Given the head of a linked list, remove the nth node from the end of the list and return its head.

**思路**：这题也是二刷，自己写出来了。先遍历得`size`，否则倒数第几个这件事情对于单链表来说很难搞。然后就是常规的`deleteAtIndex()`的写法

代码如下：
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        dummy_head = ListNode(next = head)
        cur = head
        size = 0

        while cur:
            cur = cur.next
            size += 1
        
        if head is None or n < 0 or n > size:
            return head
        
        cur = dummy_head
        for i in range(size - n):
            cur = cur.next
        if cur.next.next == None:
            cur.next = None
        else:
            cur.next = cur.next.next

        return dummy_head.next

```

## Leetcode 面试题 02.07. 链表相交 
题目链接：[面试题 02.07. 链表相交](https://leetcode.com/problems/intersection-of-two-linked-lists/)

Given the heads of two singly linked-lists `headA` and `headB`, return the node at which the two lists intersect. If the two linked lists have no intersection at all, return `null`.

For example, the following two linked lists begin to intersect at node `c1`:

![160_example_1_1](https://github.com/yukiy927/lcdiary.github.io/blob/main/pics/160_example_1_1.png)

The test cases are generated such that there are no cycles anywhere in the entire linked structure.

**Note** that the linked lists must retain their original structure after the function returns.

**Custom Judge:**

The inputs to the **judge** are given as follows (your program is not given these inputs):

`intersectVal `- The value of the node where the intersection occurs. This is 0 if there is no intersected node.

`listA `- The first linked list.

`listB `- The second linked list.

`skipA `- The number of nodes to skip ahead in `listA` (starting from the head) to get to the intersected node.

`skipB `- The number of nodes to skip ahead in `listB` (starting from the head) to get to the intersected node.

The judge will then create the linked structure based on these inputs and pass the two heads, `headA` and `headB` to your program. If you correctly return the intersected node, then your solution will be **accepted**.

**思路**：其实涉及到两个链表的时候我想到应该用双指针来解，可是<u>关键点在于如何同时移动两个指针以保证能够在intersection处相遇呢</u>？这就难倒我了，一直没有想出来，后来看了卡哥的解析，**尾端对齐**，太聪明了，如果是生活中的很具象的例子大家肯定都能想到把尾部对齐，但是抽象成文字和代码，就很难想到。所以这里又一次体现了画图的重要性啊！

画图会更加直观，就像两排麻将码在桌上，看着看着你就知道怎么办了。

方法一：暴力解法，时间复杂度O(n^2)——超时，卡在20001的一个测试case上了。

第一次写，自己想的，思路是对的，只是很耗时，这是第一次遇到无bug但超时了的，思路就是通过把第一个链表中的每一个节点指针和第二个链表的每一个节点指针比较。
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        dummy_headA = ListNode(next = headA)
        dummy_headB = ListNode(next = headB) 
        curA = dummy_headA
        curB = dummy_headB

        if headA is None or headB is None:
            return None

                    
        while curA:
            curB = dummy_headB
            while curB:
                if curA.next == curB.next:
                    return curA.next
                else:
                    curB = curB.next
            curA = curA.next

        return None 
```
方法二：双指针法（看了卡哥的解析才写出来）

**思路**：
1. 先找出两个链表的长度，进行比较，默认更长的是A链表。
2. 这个时候让指针a去找指针b，也就是尾端对齐i，这个时候a就可以和b两个指针同时对齐的位移了。
3. 为什么可以用尾端对齐呢？
    - 因为单链表始终指向唯一一个后续节点，也就是说，一旦二者相交，就会一直相交到结尾，所以从相交的起始节点开始到末尾，A和B链表的长度都是相等的。

还有一个小巧思，避免讨论情况，比较完二者长度之后，默认cura是更长的链表，让cura去找headb。
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        cura = headA
        curb = headB
        lena, lenb = 0, 0
        while cura:
            cura = cura.next
            lena += 1
        while curb:
            curb = curb.next
            lenb += 1
        cura = headA
        curb = headB
        if lena < lenb:
            cura, curb = curb, cura
            lena, lenb = lenb, lena
        
        sub = lena - lenb

        for i in range(sub):
            cura = cura.next
        while curb:
            if cura == curb:
                return cura
            else:
                cura = cura.next
                curb = curb.next
            
        return None
```
方法三：双指针+函数构造，免去重复代码段：

第一次构造函数，注意了几个地方的写法：

- 一是函数的调用参数，要确定类型
- 二是调用的时候需要加上self.blabla()，具体为什么没有细究，以后遇到再说吧，好累。
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        cura = headA
        curb = headB
        lena = self.getLength(headA)
        lenb = self.getLength(headB)
        
        if lena > lenb:
            cura = self.moveForward(headA, lena - lenb)
        else:
            curb = self.moveForward(headB, lenb - lena)
        
        while cura and curb:
            if cura == curb:
                return cura
            cura = cura.next
            curb = curb.next
            
        return None
    
    def getLength(self, head: ListNode) -> int:
        length = 0
        while head:
            head = head.next
            length += 1
        return length
    
    def moveForward(self,head: ListNode, subl: int) -> ListNode:
        while subl:
            head = head.next
            subl -= 1
        return head
```
## Leetcode 142 环形链表II 
题目链接：[142. 环形链表II ](https://leetcode.com/problems/linked-list-cycle-ii/)

Given the `head` of a linked list, return the node where the cycle begins. If there is no cycle, return `null`.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to **(0-indexed)**. It is `-1` if there is no cycle. Note that `pos` **is not passed as a parameter**.

**Do not modify** the linked list.

**思路**：昨天真的是问了一个很好的问题，环形链表，以及什么时候会出现环形，以及环形如何去判断，环形起点如何去找，这篇博客[如何判断链表中是否有环并找出环的入口位置](https://www.cnblogs.com/lonely-wolf/p/15773656.html) 全部概括到了。

我这里写的是快慢双指针法，
1. 判断是否成环
    - 快慢双指针，快指针`fast`一次走两步，`slow`一次走一步。
    - 如果成环的话，在`slow`还没走完一圈的时候，`fast`一定会和他相遇。
        - 因为`fast`以每秒一步的距离在和`slow`缩短距离，所以不会错过他而造成相遇不到。
2. 判断起点位置
    - 引入第三个指针`ptr`
    - a = (n-1)*(b+c)+c
        - 也就是说，从相遇的那一刻开始，`slow`再走若干圈个环再加上从相遇点到起始点的距离，就会和从`head`出发的`ptr`相遇，相遇点也就是环的起始点。


```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        fast, slow = head, head
        
        if head is None:
            return None
        
        while fast and fast.next:
            fast = fast.next.next
            slow = slow.next

            if fast == slow:
                ptr = head
                while ptr != slow:
                    ptr = ptr.next
                    slow = slow.next
                return ptr

        return None
```

## 总结

时长：三个小时

明天准备养成写comment的习惯了，明天刚好休息，准备把数据结构的基础知识补一下，今天就这样。
