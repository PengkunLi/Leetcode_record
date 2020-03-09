# Leetcode_record
:facepunch: To get the offer of :penguin: and :smile:
hhh
## REVIEWED PROBLEMS: 18

## [1. Two Sum](https://leetcode-cn.com/problems/two-sum/)

### Method 1 Hashmap
```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        # method 1 using hash map
        num_pair = {}
        for i, num in enumerate(nums):
            if target - num in num_pair: #line 6
                return [num_pair[target - num], i]
            num_pair[num] = i #line 8
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
        # method 1 Sliding Window
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

## [11. Container With Most Water](https://leetcode-cn.com/problems/container-with-most-water/)

### Method 1 Two pointer
```python
class Solution:
    def maxArea(self, height: List[int]) -> int:
    	# method 1 Two pointer
        left, right, water = 0, len(height) - 1, 0
        while left < right:
            if height[left] < height[right]:
                water = max(water, height[left] * (right - left))
                left += 1
            else:
                water = max(water, height[right] * (right - left))
                right -= 1
        return water
```
- We can prove that if we only move the shorter pointer, it is possible that the container can hold more water.

## [14. Longest Common Prefix](https://leetcode-cn.com/problems/longest-common-prefix/)

### Method 1 Two pointer
```python
class Solution:
    def longestCommonPrefix(self, strs: List[str]) -> str:
        # method 1 zipping
        s = ""
        for i in zip(*strs):
            if len(set(i)) == 1:
                s += i[0]
            else:
                break           
        return s 
```

### Method 2 Flat Search
```python
class Solution:
    def longestCommonPrefix(self, strs: List[str]) -> str:
        # Method 2 flat searching
        if len(strs) == 0: return ''
        s = strs[0]
        for i in range(1, len(strs)):
            while strs[i].find(s) != 0 :
                s = s[:-1]
		if not s: return s
        return s
```

## [15. 3Sum](https://leetcode-cn.com/problems/3sum/)

### Method 1 Sorting + Two pointer
```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        nums.sort()
        res = []
        for k in range(len(nums) - 2):
            if nums[k] > 0: break # the number after nums[k] must > 0, break
            if k > 0 and nums[k] == nums[k - 1]: continue 
            i, j = k + 1, len(nums) - 1
            while i < j:
                if nums[i] + nums[j] + nums[k] == 0:
                    res.append([nums[i], nums[j], nums[k]])
                    i += 1
                    j -= 1
                    while nums[i] == nums[i-1] and i < j:i += 1  # jump for the same nums
                    while nums[j] == nums[j+1] and i < j:j -= 1  # jump for the same nums
                elif nums[i] + nums[j] + nums[k] < 0:
                    i += 1
                    while nums[i] == nums[i-1] and i < j:i += 1 
                else:
                    j -= 1
                    while nums[j] == nums[j+1] and i < j:j -= 1
        return res
```

## [16. 3Sum Closest](https://leetcode-cn.com/problems/3sum-closest/)
```python
class Solution:
    def threeSumClosest(self, nums: List[int], target: int) -> int:
        n = len(nums)
        if n <= 3: return sum(nums)
        nums.sort()
        closest = sum(nums[:3])
        for i in range(n - 2):
            left, right = i + 1, n - 1
            while left < right:
                candi = nums[i] + nums[left] + nums[right]
                if abs(closest - target) > abs(candi - target):
                    closest = candi
                if candi < target:
                    left += 1
                elif candi > target:
                    right -= 1
                else:
                    return closest
        return closest
```

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

## [20. Valid Parentheses](https://leetcode-cn.com/problems/valid-parentheses/submissions/)
### Method1 stack
```python
class Solution:
    def isValid(self, s: str) -> bool:
        if not s: return True
        lookup = {')' : '(',
                  ']' : '[',
                  '}' : '{'}

        stack = []
        for par in s:
            if par not in lookup:
                stack.append(par)
            elif stack and stack[-1] == lookup[par]:
                stack.pop()
            else:
                return False
        return not len(stack)
```
- Should pay attention to every single case and list each of them on the paper.

## [24. Swap Nodes in Pairs]()
### Method 1 Dummy Node
```python
class Solution:
    def swapPairs(self, head: ListNode) -> ListNode:
        if not head or not head.next: return head
        dum = ListNode(-1)
        dum.next = head
        first, second = dum, dum.next
        while first.next and second.next:
            p = ListNode(0)
            p.next = first.next
            first.next = second.next
            p.next.next  = second.next.next
            first.next.next = p.next
            p = None
            second = second.next
            first = first.next.next
        return dum.next
```
- Should draw nodes sequence to illustrate
## [27. Remove Element](https://leetcode-cn.com/problems/remove-element/submissions/)
### Method 1 Two Pointers
```python
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        left, right = 0, len(nums) - 1
        if left == right and nums[left] == val or not nums:
            return 0
        while left <= right:
            if nums[left] == val:
                nums[left], nums[right] = nums[right], nums[left]
                right -= 1
            else:
                left += 1
        return right + 1
```
- Need to carefully consider the termination condition of Two pointers

## [28. Inplement strStr()](https://leetcode-cn.com/problems/remove-element/submissions/)
### Method1 Brute Force
```python
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        m, n = len(haystack), len(needle)
        i = 0
        if not n: return 0
        if not m or m < n: return -1
        while i + n <= m:
            if haystack[i:i+n] == needle:
                return i
            i += 1
        return -1
```

- Other Methods like KMP, Sunday, [Offset Table](https://leetcode-cn.com/problems/implement-strstr/solution/python3-sundayjie-fa-9996-by-tes/)


## [33. Search in Rotated Sorted Array](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/)

### Method1 Binary Search
```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        if len(nums) == 0: return -1
        start, end = 0, len(nums) - 1
        while start + 1 < end:
            mid = start + (end - start) // 2;
            if nums[mid] > nums[start]:
                if nums[start] <= target <= nums[mid]:
                    end = mid
                else:
                    start = mid
            else:
                if nums[mid] <= target <= nums[end]:
                    start = mid
                else:
                    end = mid
        if nums[start] == target:
            return start
        elif nums[end] == target:
            return end
        return -1
```

## [34. Find First and Last Position of Element in Sorted Array](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

### Method 1 Binary Search
```python
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        if len(nums) == 0: return [-1, -1]
        def searchLeft(nums, target):
            start, end = 0, len(nums) - 1
            while start + 1 < end:
                mid = start + (end - start) // 2
                if nums[mid] >= target:
                    end = mid
                else:
                    start = mid
            if nums[start] == target:
                return start
            if nums[end] == target:
                return end
            return -1
        def searchRight(nums, target):
            start, end = 0, len(nums) - 1
            while start + 1 < end:
                mid = start + (end - start) // 2
                if nums[mid] <= target:
                    start = mid
                else:
                    end = mid
            if nums[end] == target:
                return end
            if nums[start] == target:
                return start
            return -1
        return [searchLeft(nums, target), searchRight(nums, target)]
```

## [35. Search Insert Position](https://leetcode-cn.com/problems/search-insert-position/)

### Method1 Binary Search
```python
class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:
        # find the first position of a number that is bigger or equal tp the target
        n = len(nums)
        start, end = 0, n - 1
        if not nums or target <= nums[0]:
            return 0
        while start + 1 < end:
            mid = start + (end - start) // 2
            if nums[mid] == target:
                return mid
            elif nums[mid] > target:
                end = mid
            elif nums[mid] < target:
                start = mid
        if nums[start] == target:
            return start
        if nums[end] >= target:
            return end
        return n
```

## [81. Search in Rotated Sorted Array](https://leetcode-cn.com/problems/search-in-rotated-sorted-array-ii/)
```python
class Solution:
    def search(self, nums: List[int], target: int) -> bool:
        if len(nums) == 0: return False
        l, r = 0, len(nums) - 1

        while l + 1 < r:
            mid = l + (r - l) // 2
            if nums[mid] == target: return True
            # mid on the left part
            if nums[mid] > nums[l]:
                if nums[l] <= target < nums[mid]:
                    r = mid  
                else:
                    l = mid
            # mid on the right part
            elif nums[mid] < nums[l]:
                if nums[mid] < target <= nums[r]:
                    l = mid
                else:
                    r = mid
            # not sure
            else:
                l += 1
        if nums[l] == target or nums[r] == target:
            return True
            
        return False
```
- Worst case is O(n), avg O(log n)

## [153. Find Minimum in Rotated Sorted Array](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/)
### Method1 Binary Search
```python
class Solution:
    def findMin(self, nums: List[int]) -> int:
        if len(nums) == 0: return -1
        start, end = 0, len(nums) - 1

        while start + 1 < end:
            mid = start + (end - start) // 2;
            if nums[mid] > nums[start]:
                start = mid
            else:
                end = mid
        return min(nums[start], nums[end], nums[0])
```

## [154. Find Minimum in Rotated Sorted Array II](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array-ii/)
### Method1 Tricky
```python
class Solution:
    def findMin(self, nums: List[int]) -> int:
        left, right = 0, len(nums) - 1
        while left < right:
            mid = left + (right - left) // 2
            if nums[mid] > nums[right]: left = mid + 1
            elif nums[mid] < nums[right]: right = mid
            else: right = right - 1 # key
        return nums[left]
```

### Method2 Template
```python
class Solution:
    def findMin(self, nums: List[int]) -> int:
        if len(nums) == 0: return -1
        start, end = 0, len(nums) - 1

        while start + 1 < end:
            mid = start + (end - start) // 2;
            if nums[mid] > nums[end]:
                start = mid
            elif nums[mid] < nums[end]:
                end = mid
            else:
                end -= 1
     
        return min(nums[start], nums[end])
```
- can only compare with the end

## [200. Number of islands](https://leetcode-cn.com/problems/number-of-islands/)

### Method1 Iteration BFS/DFS
```python
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        moves = [(1,0),(0,1),(-1,0),(0,-1)]

        m = len(grid)
        if not m: return 0 
        n = len(grid[0])
        if not n: return 0
        visited = set()
        islands = 0

        for i in range(m):
            for j in range(n):
                if grid[i][j] == '1' and (i,j) not in visited:
                    islands += 1
                    visited.add((i,j))
                    deque = [(i,j)]
                    while deque:
                        lasti, lastj = deque.pop(0)
			#lasti, lastj = deque.pop()
                        for move in moves:
                            curi, curj = lasti + move[0], lastj + move[1]
                            if -1 < curi < m and -1 < curj < n and (curi,curj) not in visited:
                                visited.add((curi,curj))
                                if grid[curi][curj] == '1':
                                    deque.append((curi, curj))
                                    
        return islands
```

### Method2 Recursion DFS
```python
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        self.moves = [(1,0),(0,1),(-1,0),(0,-1)]

        m = len(grid)
        if not m: return 0 
        n = len(grid[0])
        if not n: return 0
        visited = set()
        islands = 0

        for i in range(m):
            for j in range(n):
                if grid[i][j] == '1' and (i,j) not in visited:
                    islands += 1
                    self.__dfs(grid, i, j, m, n, visited)           
        return islands
    
    def __dfs(self, grid, lasti, lastj, m, n, visited):
        visited.add((lasti,lastj))
        for move in self.moves:
            curi, curj = lasti + move[0], lastj + move[1]
            if -1 < curi < m and -1 < curj < n and (curi,curj) not in visited:
                if grid[curi][curj] == '1':
                    self.__dfs(grid, curi, curj, m, n, visited)
```
