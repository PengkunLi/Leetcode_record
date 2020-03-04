# Linked List

target 2,19,21,23,24,25,61,82,83,86

## [19. Remove Nth Node From End of List](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)
```python
class Solution:
    def removeNthFromEnd(self, head: ListNode, n: int) -> ListNode:
        if not head: return head
        if not head.next and n == 1: return None
        dum = ListNode(-1)
        dum.next = head
        fast = slow = dum #pay more attention
        while n:
            fast = fast.next
            n -= 1
        while fast and fast.next:
            slow = slow.next
            fast = fast.next
        slow.next = slow.next.next
        return dum.next
```
- Should pay more attention to the position of the fast and slow pointer.

## [21. Merge Two Sorted Lists](https://leetcode-cn.com/problems/merge-two-sorted-lists/)
### Method 1 recursion
```python
class Solution:
    def mergeTwoLists(self, l1: ListNode, l2: ListNode) -> ListNode:
        if not l1:
            return l2
        elif not l2:
            return l1
        elif l1.val < l2.val:
            l1.next = self.mergeTwoLists(l1.next, l2)
            return l1
        else:
            l2.next = self.mergeTwoLists(l1, l2.next)
            return l2
```
### Method 2 iteration
```python
class Solution:
    def mergeTwoLists(self, l1, l2):
        # maintain an unchanging reference to node ahead of the return node.
        prehead = ListNode(-1)

        prev = prehead
        while l1 and l2:
            if l1.val <= l2.val:
                prev.next = l1
                l1 = l1.next
            else:
                prev.next = l2
                l2 = l2.next            
            prev = prev.next

        # exactly one of l1 and l2 can be non-null at this point, so connect
        # the non-null list to the end of the merged list.
        prev.next = l1 if l1 is not None else l2

        return prehead.next
```

## [141. Linked List Cycle](https://leetcode-cn.com/problems/linked-list-cycle/)
```python
class Solution:
    def hasCycle(self, head: ListNode) -> bool:
        if not head:
            return False
        slow, fast = head, head.next 
        while slow != fast:
            if not fast or not fast.next:
                return False
            slow = slow.next 
            fast = fast.next.next 
        return True
```

## [142. Linked List Cycle II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)
```python
class Solution:
    def detectCycle(self, head: ListNode) -> ListNode:
        fast, slow = head, head
        while True:
            if not (fast and fast.next): return
            fast, slow = fast.next.next, slow.next
            if fast == slow: break
        fast = head
        while fast != slow:
            fast, slow = fast.next, slow.next
        return fast
```

## [148. Sort List](https://leetcode-cn.com/problems/sort-list/)

```python
class Solution:
    def sortList(self, head: ListNode) -> ListNode:
        dummy = ListNode(-1)
        dummy.next = head
        n = self.listLen(head)
        step = 1
        while step < n:
            cur = dummy.next
            tal = dummy
            while cur:
                left = cur
                right = self.cutNLengthList(left, step)
                cur = self.cutNLengthList(right, step)
                tal.next = self.mergeTwoLists(left, right)
                while tal.next: tal = tal.next
            step *= 2
        return dummy.next

    def listLen(self, p):
        n = 0
        while p:
            p = p.next
            n += 1
        return n

    def cutNLengthList(self, head, n):
        p = head
        while n > 1 and p:
            p = p.next
            n -= 1
        if not p:
            return p
        else:
            n = p.next
            p.next = None
            return n

    def mergeTwoLists(self, l1, l2):
        # maintain an unchanging reference to node ahead of the return node.
        prehead = ListNode(-1)

        prev = prehead
        while l1 and l2:
            if l1.val <= l2.val:
                prev.next = l1
                l1 = l1.next
            else:
                prev.next = l2
                l2 = l2.next            
            prev = prev.next

        # exactly one of l1 and l2 can be non-null at this point, so connect
        # the non-null list to the end of the merged list.
        prev.next = l1 if l1 is not None else l2

        return prehead.next
```

## [206. Reverse Linked List](https://leetcode-cn.com/problems/reverse-linked-list/)

### Method1 Recursion
```python
class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        if not head or not head.next: return head
        p = self.reverseList(head.next)
        head.next.next = head
        head.next = None
        return p
```

### Method2 Iteration
```python
class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        pre = None
        cur = head
        while cur != None:
            tmp = cur.next
            cur.next = pre
            pre = cur
            cur = tmp
        return pre
```

