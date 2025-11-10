# Pattern: Two Pointers - Phase 1

## Problem: 167. Two Sum II - Input Array is Sorted

Given a 1-indexed array of integers `numbers` that is **already sorted** in non-decreasing order, find two numbers such that they add up to a specific `target` number. Let these two numbers be `numbers[index1]` and `numbers[index2]` where `1 <= index1 < index2 <= numbers.length`.

Return the indices of the two numbers, `index1` and `index2`, added by one as an integer array `[index1, index2]` of length 2.

The tests are generated such that there is exactly one solution. You may not use the same element twice.

Your solution must use only constant extra space.

### Example 1

```
Input: numbers = [2,7,11,15], target = 9
Output: [1,2]
Explanation: The sum of 2 and 7 is 9. Therefore, index1 = 1, index2 = 2. We return [1, 2].
```

### Example 2

```
Input: numbers = [2,3,4], target = 6
Output: [1,3]
Explanation: The sum of 2 and 4 is 6. Therefore index1 = 1, index2 = 3. We return [1, 3].
```

### Example 3

```
Input: numbers = [-1,0], target = -1
Output: [1,2]
Explanation: The sum of -1 and 0 is -1. Therefore index1 = 1, index2 = 2. We return [1, 2].
```

### Constraints

- `2 <= numbers.length <= 3 * 10^4`
- `-1000 <= numbers[i] <= 1000`
- `numbers` is sorted in non-decreasing order.
- `-1000 <= target <= 1000`
- The tests are generated such that there is exactly one solution.

### Solution

**Logic:**

**Code:**

```python
class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        i, j = 0, len(numbers)-1
        while i != j:
            if numbers[i] + numbers[j] < target:
                i += 1
            elif numbers[i] + numbers[j] > target:
                j -= 1
            else:
                return [i+1, j+1]
```

## Problem: 125. Valid Palindrome

A phrase is a palindrome if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward. Alphanumeric characters include letters and numbers.

Given a string `s`, return `true` if it is a palindrome, or `false` otherwise.

### Example 1

```
Input: s = "A man, a plan, a canal: Panama"
Output: true
Explanation: "amanaplanacanalpanama" is a palindrome.
```

### Example 2

```
Input: s = "race a car"
Output: false
Explanation: "raceacar" is not a palindrome.
```

### Example 3

```
Input: s = " "
Output: true
Explanation: s is an empty string "" after removing non-alphanumeric characters.
Since an empty string reads the same forward and backward, it is a palindrome.
```

### Constraints

- `1 <= s.length <= 2 * 10^5`
- s consists only of printable ASCII characters.

### Solution

**Logic:**

**Code:**

```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        s = s.lower()
        s = ''.join(char for char in s if char.isalnum())
        i, j = 0, len(s)-1
        while i < j:
            if s[i] != s[j]:
                return False
            i += 1
            j -= 1
        return True
```

## Problem: 680. Valid Palindrome II

Given a string `s`, return `true` if the `s` can be palindrome after deleting at most one character from it.

### Example 1

```
Input: s = "aba"
Output: true
```

### Example 2

```
Input: s = "abca"
Output: true
Explanation: You could delete the character 'c'.
```

### Example 3

```
Input: s = "abc"
Output: false
```

### Constraints

- `1 <= s.length <= 105`
- `s` consists of lowercase English letters.

### Solution

**Logic:**

**Code:**

```python
class Solution:
    def validPalindrome(self, s: str) -> bool:
        def check(i, j):
            while i < j:
                if s[i] != s[j]:
                    return False
                i += 1
                j -= 1
            return True

        i, j = 0, len(s) - 1
        while i < j:
            if s[i] != s[j]:
                return check(i+1, j) or check(i, j-1)
            i += 1
            j -= 1
        return True
```

## Problem: 11. Container with Most Water

You are given an integer array `height` of length `n`. There are `n` vertical lines drawn such that the two endpoints of the `ith` line are `(i, 0)` and `(i, height[i])`.

Find two lines that together with the x-axis form a container, such that the container contains the most water.

Return the maximum amount of water a container can store.

### Example 1

```
Input: height = [1,8,6,2,5,4,8,3,7]
Output: 49
Explanation: The above vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the container can contain is 49.
```

### Example 2

```
Input: height = [1,1]
Output: 1
```

### Constraints

- `n == height.length`
- `2 <= n <= 10^5`
- `0 <= height[i] <= 10^4`

### Solution

**Logic:**

**Code:**

```python
class Solution:
    def maxArea(self, height: List[int]) -> int:
        i, j = 0, len(height) - 1
        res = 0
        while i < j:
            area = min(height[i], height[j]) * (j-i)
            res = max(res, area)
            if height[i] < height[j]:
                i += 1
            else:
                j -= 1
        return res
```

## Problem: 977. Squares of a Sorted Array

Given an integer array `nums` sorted in non-decreasing order, return an array of the squares of each number sorted in non-decreasing order.

### Example 1

```
Input: nums = [-4,-1,0,3,10]
Output: [0,1,9,16,100]
Explanation: After squaring, the array becomes [16,1,0,9,100].
After sorting, it becomes [0,1,9,16,100].
```

### Example 2

```
Input: nums = [-7,-3,2,3,11]
Output: [4,9,9,49,121]
```

### Constraints

- `1 <= nums.length <= 10^4`
- `-10^4 <= nums[i] <= 10^4`
- `nums` is sorted in non-decreasing order.

### Solution

**Logic:**

**Code:**

```python
class Solution:
    def sortedSquares(self, nums: List[int]) -> List[int]:
        i, j = 0, len(nums) - 1
        res = []
        while i < j:
            if abs(nums[i]) > abs(nums[j]):
                res.append(nums[i] ** 2)
                i += 1
            else:
                res.append(nums[j] ** 2)
                j -= 1
        res.append(nums[i] ** 2)
        return res[::-1]
```
