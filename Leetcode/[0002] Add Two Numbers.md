
# <p align="center"> 2. Add Two Numbers</p>

## 1. Problem:
You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.
You may assume the two numbers do not contain any leading zero, except the number 0 itself.

给出两个 非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 逆序 的方式存储的，并且它们的每个节点只能存储 一位 数字。
如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。
您可以假设除了数字 0 之外，这两个数都不会以 0 开头。

## 2. Example:
Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)  
Output: 7 -> 0 -> 8  
Explanation: 342 + 465 = 807.  

## 3. Tags:
Linked List, Math

## 4. Solutions:

### 1. Elementary Math
1. 初始化进位位carry，构建result的头节点
2. 当两个链表不同时为空时，循环读取两个链表的首元素$l1, l2$：
3. &emsp; 若链表不为空，取其值；若链表为空，将其值置为0
4. &emsp; 求和：$x + y + carry$，若结果大于10，则结果减10，进位位置1，否则进位位置0
5. &emsp; 若链表不为空，更新链表头指针，l = l -> next, 更新result = result -> next
6. 若两个链表均为空，则判断进位位，若carry = 1，增加节点ListNode(1)，返回result.next

复杂度分析：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$


```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        carry = 0
        res = p = ListNode(-1)
        while l1 or l2:
            x = l1.val if l1 else 0
            y = l2.val if l2 else 0
            num = x + y + carry
            if num >= 10: num, carry = num-10, 1
            else: carry = 0
            p.next = ListNode(num)
            p = p.next
            if l1: l1 = l1.next
            if l2: l2 = l2.next   
        if carry: p.next = ListNode(1)
        return res.next
```
