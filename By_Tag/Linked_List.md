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

