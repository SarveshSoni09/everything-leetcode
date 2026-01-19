#

## Problem: 704. Binary Search

Given an array of integers `nums` which is sorted in ascending order, and an integer `target`, write a function to search `target` in `nums`. If `target` exists, then return its index. Otherwise, return `-1`.

You must write an algorithm with `O(log n)` runtime complexity.

### Example 1

```
Input: nums = [-1,0,3,5,9,12], target = 9
Output: 4
Explanation: 9 exists in nums and its index is 4
```

### Example 2

```
Input: nums = [-1,0,3,5,9,12], target = 2
Output: -1
Explanation: 2 does not exist in nums so return -1
```

### Constraints

- `1 <= nums.length <= 10^4`
- `-104 < nums[i], target < 10^4`
- All the integers in `nums` are unique.
- `nums` is sorted in ascending order.

### Solution

**Logic:**

**Code:**

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        l, r = 0, len(nums) - 1
        while l <= r:
            mid = (l + r) // 2
            if nums[mid] == target:
                return mid
            elif nums[mid] > target:
                r = mid - 1
            else:
                l = mid + 1
        return -1
```

## Problem: 35. Search Insert Position

Given a sorted array of distinct integers and a `target` value, return the index if the `target` is found. If not, return the `index` where it would be if it were inserted in order.

You must write an algorithm with `O(log n)` runtime complexity.

### Example 1

```
Input: nums = [1,3,5,6], target = 5
Output: 2
```

### Example 2

```
Input: nums = [1,3,5,6], target = 2
Output: 1
```

### Example 3

```
Input: nums = [1,3,5,6], target = 7
Output: 4
```

### Constraints

- `1 <= nums.length <= 10^4`
- `-10^4 <= nums[i] <= 10^4`
- `nums` contains distinct values sorted in ascending order.
- `-104 <= target <= 10^4`

### Solution

**Logic:**

**Code:**

```python
class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:
        l, r = 0, len(nums) - 1
        while l <= r:
            mid = (l + r) // 2
            if nums[mid] == target:
                return mid
            elif nums[mid] > target:
                r = mid - 1
            else:
                l = mid + 1
        return l
```

## Problem: 34. Find First and Last Position of Element in Sorted Array

Given an array of integers `nums` sorted in non-decreasing order, find the starting and ending position of a given `target` value.

If target is not found in the array, return `[-1, -1]`.

You must write an algorithm with `O(log n)` runtime complexity.

### Example 1

```
Input: nums = [5,7,7,8,8,10], target = 8
Output: [3,4]
```

### Example 2

```
Input: nums = [5,7,7,8,8,10], target = 6
Output: [-1,-1]
```

### Example 3

```
Input: nums = [], target = 0
Output: [-1,-1]
```

### Constraints

- `0 <= nums.length <= 10^5`
- `-10^9 <= nums[i] <= 10^9`
- `nums` is a non-decreasing array.
- `-10^9 <= target <= 10^9`

### Solution

**Logic:**

**Code:**

```python
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        l, r = 0, len(nums) - 1
        res = [-1, -1]
        while l <= r:
            mid = (l + r) // 2
            if nums[mid] == target:
                l = r = mid
                while l >= 0 and nums[l] == target:
                    l -= 1
                while r < len(nums) and nums[r] == target:
                    r += 1
                res = [l + 1, r - 1]
                return res
            elif nums[mid] > target:
                r = mid - 1
            else:
                l = mid + 1
        return res
```

## Problem: 74. Search 2D Matrix

You are given an `m x n` integer matrix `matrix` with the following two properties:

- Each row is sorted in non-decreasing order.
- The first integer of each row is greater than the last integer of the previous row.

Given an integer `target`, return `true` if `target` is in `matrix` or `false` otherwise.

You must write a solution in `O(log(m * n))` time complexity.

### Example 1

```
Input: matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 3
Output: true
```

### Example 2

```
Input: matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 13
Output: false
```

### Constraints

- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= m, n <= 100`
- `-10^4 <= matrix[i][j], target <= 10^4`

### Solution

**Logic:**

**Code:**

```python
class Solution:
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        m, n = len(matrix), len(matrix[0])
        l, r = 0, (m * n) - 1
        while l <= r:
            mid = (l + r) // 2
            row = mid // n
            col = mid % n
            if matrix[row][col] == target:
                return True
            elif matrix[row][col] > target:
                r = mid - 1
            else:
                l = mid + 1
        return False
```

## Problem: 33. Search in Rotated Sorted Array

There is an integer array `nums` sorted in ascending order (with distinct values).

Prior to being passed to your function, `nums` is possibly left rotated at an unknown index `k (1 <= k < nums.length)` such that the resulting array is `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]` (0-indexed). For example, `[0,1,2,4,5,6,7]` might be left rotated by `3` indices and become `[4,5,6,7,0,1,2]`.

Given the array `nums` after the possible rotation and an integer `target`, return the index of `target` if it is in `nums`, or `-1` if it is not in `nums`.

You must write an algorithm with `O(log n)` runtime complexity.

### Example 1

```
Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4
```

### Example 2

```
Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1
```

### Example 3

```
Input: nums = [1], target = 0
Output: -1
```

### Constraints

- `1 <= nums.length <= 5000`
- `-10^4 <= nums[i] <= 10^4`
- All values of `nums` are unique.
- `nums` is an ascending array that is possibly rotated.
- `-10^4 <= target <= 10^4`

### Solution

**Logic:**

**Code:**

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        l, r = 0, len(nums) - 1
        while l <= r:
            mid = (l + r) // 2
            if nums[mid] == target:
                return mid
            if nums[l] <= nums[mid]:
                if target > nums[mid] or target < nums[l]:
                    l = mid + 1
                else:
                    r = mid - 1
            else:
                if target < nums[mid] or target > nums[r]:
                    r = mid - 1
                else:
                    l = mid + 1
        return -1
```
