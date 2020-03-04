# Sorting Algorithms

## Merge Sorting

### [148. Sort List](https://leetcode-cn.com/problems/sort-list/)

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

## Bucket Sorting

### [154. Maximum gap](https://leetcode-cn.com/problems/maximum-gap/)
```python
class Solution:
    def maximumGap(self, nums: List[int]) -> int:
        n = len(nums)
        if n < 2: return 0
        max_num = max(nums)
        min_num = min(nums)
        gap = math.ceil((max_num - min_num)/(n - 1))
        bucket = [[float("inf"), float("-inf")] for _ in range(n - 1)]
        
        # keep the min/max val of each bucket
        for num in nums:
            if num == max_num or num == min_num:
                continue
            loc = (num - min_num) // gap
            bucket[loc][0] = min(num, bucket[loc][0])
            bucket[loc][1] = max(num, bucket[loc][1])

        preMin = min_num
        res = float("-inf")
        for x, y in bucket:
            if x == float("inf") :
                continue
            res = max(res, x - preMin)
            preMin = y
        res = max(res, max_num - preMin)
        return res
```

