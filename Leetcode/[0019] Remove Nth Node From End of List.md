
# <p align="center"> 19. Remove Nth Node From End of List</p>

## 1. Problem:
Given a linked list, remove the n-th node from the end of list and return its head.  
Note:
Given n will always be valid.  
Follow up:
Could you do this in one pass?

给定一个链表，删除链表的倒数第 n 个节点，并且返回链表的头结点。

## 2. Example:
Given linked list: 1->2->3->4->5, and n = 2.  
After removing the second node from the end, the linked list becomes 1->2->3->5.

## 3. Tags:
Linked List, Two Pointers

## 4. Solutions:

### 1. Two pass algorithm
思路：删除倒数第n个节点，意味着删除正数第$L-n+1$个节点，问题转换为求链表的长度，然后再一次遍历删除该节点，

### 2. One pass algorithm
思路：设置两个指针fast与slow，fast先移至第n个节点处，此时开始同时移动fast与slow两个指针，当fast到达链表末位时，slow刚好指向倒数第n个节点。


```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def removeNthFromEnd(self, head, n):
        """
        :type head: ListNode
        :type n: int
        :rtype: ListNode
        """
        slow, fast = head, head
        for _ in range(n):
            fast = fast.next
        if not fast: return head.next
        while fast.next:
            slow = slow.next
            fast = fast.next
        slow.next = slow.next.next
        return head
```
