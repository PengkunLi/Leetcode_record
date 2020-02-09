# Leetcode_record
:facepunch: To get the offer of :penguin: and :smile:

## [1. Two Sum](https://leetcode-cn.com/problems/two-sum/)

### Method 1 Hashmap
```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        # method 1 using hash map
        num_pair = {}
        for i, num in enumerate(nums):
            if target - num in num_pair:
                return [num_pair[target - num], i]
            num_pair[num] = i       
        return None
```
Logic Between line 6 and line 8:

1. We have to know that we are recording number and its index.
2. So we need to check if its complement is in the hash map first.
3. Once there is a complement in the hash map, we return their index respectively.
4. Else we record num and its index.
5. If line 8 is prior to line 6 then if e.g. [1,1,4,5], target = 2:
		it will return [0,0]

## [2. Add Two Numbers](https://leetcode.com/problems/add-two-numbers/)

### Method 1 Iteration
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        # method 1 iteration
        carry = 0
        root = dum = ListNode(-1) 
        while l1 or l2 or carry:
            cur = 0
            if l1:
                cur += l1.val
                l1 = l1.next
            if l2:
                cur += l2.val
                l2 = l2.next
            cur += carry
            carry = cur // 10
            cur = cur % 10
            s = ListNode(cur)
            root.next = s
            root = root.next
        return dum.next
```
- note the carry to add a new node at the end of the list

### Method 2 Recursion
```python
class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        # method 2 recursion
        def addTwoNumbershelper(l1, l2, carry = 0):
            if not (l1 or l2): return ListNode(1) if carry else None
            l1, l2 = l1 or ListNode(0), l2 or ListNode(0)
            val = l1.val + l2.val + carry
            l1.val, l1.next = val % 10, addTwoNumbershelper(l1.next, l2.next, val > 9)
            return l1
        return addTwoNumbershelper(l1, l2)
```

## [3. Longest Substring Without Repeating Characters](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)

### Method 1 Sliding window
```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        # Sliding Window
        start, maxlen, record = 0, 0, {}
        for i, ch in enumerate(s):
            if ch in record and record[ch] >= start:
                maxlen = max(maxlen, i - start)
                start = record[ch] + 1
            record[ch] = i
        return max(maxlen, len(s) - start)
```
- Think about what is the condition to update maxlen
- ```record[ch] >= start``` makes sure start moves forward and avoid execuating the if condition too many times unnecessarily.
