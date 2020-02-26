# Binary Search

## Template
```c++
int binarySearch(vector<int> &A, int target){
    if (A.size() == 0) {
        return -1;
    }
    
    int start = 0, end = A.size() - 1;
    int mid;
    
    while (start + 1 < end) {
        mid = start + (end - start) / 2;// considering overflow
        if (A[mid] == target){
            end = mid;
        } else if (A[mid] < target) {
            start = mid;
        } else if (A[mid] > target) {
            end = mid;
        }
    }
    
    if (A[start] == target) {
        return start;
    } else if (A[end] == target) {
        return end;
    } 
    
    return -1;
    
}
```
## [33. Search in Rotated Sorted Array](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/)

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

## [34. Find First and Last Position of Element in Sorted Array](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/)
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


## [153. Find Minimum in Rotated Sorted Array](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/)
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
