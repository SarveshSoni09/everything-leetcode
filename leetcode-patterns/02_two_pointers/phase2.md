# Pattern: Two Pointers - Phase 2

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

This problem extends **Problem 125 (Valid Palindrome)** by allowing **one deletion**. This introduces the concept of **backtracking** or **trying alternatives** when the standard two-pointer check fails.

1. **Building on Problem 125:** The base case is identical - use two pointers to check if characters match. However, when we find a mismatch, we have a choice: delete the character at `i` or delete the character at `j`.

2. **The Strategy:**

   - Use the standard two-pointer approach from Problem 125.
   - When `s[i] != s[j]`, we haven't failed yet - we can try deleting one character.
   - We need to check if the remaining string (after deleting either `s[i]` or `s[j]`) is a palindrome.
   - Create a helper function `check(i, j)` that verifies if `s[i:j+1]` is a palindrome.

3. **The Helper Function:** The `check` function is essentially the same as Problem 125's solution - it uses two pointers to verify if a substring is a palindrome.

4. **Key Insight:** When we find a mismatch, we try both possibilities:

   - Skip the character at `i`: check if `s[i+1:j+1]` is a palindrome
   - Skip the character at `j`: check if `s[i:j]` is a palindrome
   - If either works, return `True`.

5. **Time Complexity:** O(N) - in the worst case, we might check the string twice (once with the original pointers, once in the helper), but each character is still visited a constant number of times.
6. **Space Complexity:** O(1) - only pointers are used.

This problem demonstrates how to **extend** a two-pointer solution when we need to handle **one exception** or **alternative path**. The pattern of "try both options when a check fails" is useful in many variations.

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

This problem introduces a **greedy optimization** strategy with two pointers. Unlike previous problems where we compared sums or characters, here we're **maximizing an area** while making intelligent decisions about which pointer to move.

1. **The Problem:** We need to find two lines that form a container with maximum area. The area is `min(height[i], height[j]) * (j - i)`.

2. **Two-Pointer Setup:** Start with `i` at the beginning and `j` at the end. This maximizes the width `(j - i)` initially.

3. **The Greedy Strategy:**

   - Calculate the current area: `min(height[i], height[j]) * (j - i)`.
   - Update the maximum area seen so far.
   - **Key Insight:** Move the pointer pointing to the **shorter** line. Why?
     - The area is limited by the shorter line (we use `min(height[i], height[j])`).
     - Moving the pointer at the taller line would only decrease the width while the height can't increase (it's still limited by the shorter line).
     - Moving the shorter line might find a taller line, potentially increasing the area.

4. **Why This Works:** This greedy approach is correct because:

   - We start with maximum width.
   - At each step, we eliminate all containers that would have the current shorter line as one boundary (since moving the taller line would only make them worse).
   - We continue until the pointers meet.

5. **Time Complexity:** O(N) - each element is visited at most once.
6. **Space Complexity:** O(1) - only two pointers and a result variable.

This problem demonstrates **greedy decision-making** with two pointers. The pattern of "move the pointer that gives us a chance to improve" is fundamental to optimization problems with two pointers.

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

## Problem: 905. Sort Array By Parity

Given an integer array `nums`, move all the even integers at the beginning of the array followed by all the odd integers.

Return any array that satisfies this condition.

### Example 1

```
Input: nums = [3,1,2,4]
Output: [2,4,3,1]
Explanation: The outputs [4,2,3,1], [2,4,1,3], and [4,2,1,3] would also be accepted.
```

### Example 2

```
Input: nums = [0]
Output: [0]
```

### Constraints

- `1 <= nums.length <= 5000`
- `0 <= nums[i] <= 5000`

### Solution

**Logic:**

This problem extends the **conditional swapping** pattern from Problem 345 (Reverse Vowels). Instead of swapping vowels, we're **partitioning** the array into two groups: evens (left) and odds (right).

1. **Building on Problem 345:** Like Problem 345, we use two pointers with conditional movement. However, instead of reversing, we're **partitioning** - moving evens to the left and odds to the right.

2. **Two-Pointer Partitioning Strategy:**

   - Start with `l` at the beginning and `r` at the end.
   - **Skip elements in correct position:**
     - While `nums[l]` is even (already in correct position), increment `l`.
     - While `nums[r]` is odd (already in correct position), decrement `r`.
   - When `l < r` and we've found an even at `r` and an odd at `l`, swap them.
   - After swapping, move both pointers: `l += 1` and `r -= 1`.

3. **Key Insight:** This is essentially a **two-way partitioning** algorithm, similar to the partition step in quicksort. We maintain the invariant:

   - All elements to the left of `l` are even (or we're about to place an even there).
   - All elements to the right of `r` are odd (or we're about to place an odd there).

4. **Why This Works:** By skipping elements already in the correct position and only swapping when necessary, we efficiently partition the array in one pass.

5. **Time Complexity:** O(N) - each element is visited at most once.
6. **Space Complexity:** O(1) - only two pointers are used, modification is in-place.

This problem demonstrates **partitioning with two pointers**, which is a fundamental technique that will be extended in Problem 75 (Sort Colors) to handle three partitions.

**Code:**

```python
class Solution:
    def sortArrayByParity(self, nums: List[int]) -> List[int]:
        l, r = 0, len(nums) - 1
        while l < r:
            while nums[l] % 2 == 0 and l < r:
                l += 1
            while nums[r] % 2 == 1 and l < r:
                r -= 1
            nums[l], nums[r] = nums[r], nums[l]
            l += 1
            r -= 1
        return nums
```

## Problem: 75. Sort Colors

Given an array `nums` with `n` objects colored red, white, or blue, sort them in-place so that objects of the same color are adjacent, with the colors in the order red, white, and blue.

We will use the integers `0`, `1`, and `2` to represent the color red, white, and blue, respectively.

You must solve this problem without using the library's sort function.

### Example 1

```
Input: nums = [2,0,2,1,1,0]
Output: [0,0,1,1,2,2]
```

### Example 2

```
Input: nums = [2,0,1]
Output: [0,1,2]
```

### Constraints

- `n == nums.length`
- `1 <= n <= 300`
- `nums[i]` is either 0, 1, or 2.

Follow up: Could you come up with a one-pass algorithm using only constant extra space?

### Solution

**Logic:**

This problem extends the **partitioning** concept from Problem 905 to handle **three partitions** instead of two. It's known as the "Dutch National Flag" problem and requires **three pointers** (or two pointers plus an iterator).

1. **Building on Problem 905:** Problem 905 partitioned into two groups (evens/odds). This problem partitions into three groups (0s, 1s, 2s) in a specific order.

2. **Three-Pointer Strategy:**

   - `l` (left pointer): points to the next position where a `0` should be placed. All elements to the left of `l` are `0`s.
   - `r` (right pointer): points to the next position where a `2` should be placed. All elements to the right of `r` are `2`s.
   - `i` (iterator): scans through the array from left to right.

3. **The Algorithm:**

   - While `i <= r`:
     - If `nums[i] == 2`: Swap with `nums[r]` and decrement `r`. **Don't increment `i`** because the element swapped from `r` might be a `0` or `1` that needs processing.
     - If `nums[i] == 0`: Swap with `nums[l]` and increment both `l` and `i`. We can increment `i` because `nums[l]` was either `1` (which is correct) or `0` (which we just swapped with itself).
     - If `nums[i] == 1`: Just increment `i`. `1`s stay in the middle.

4. **Key Insight:** The invariant maintained is:

   - `[0...l-1]`: all `0`s
   - `[l...i-1]`: all `1`s
   - `[i...r]`: unprocessed elements
   - `[r+1...n-1]`: all `2`s

5. **Why We Don't Increment `i` After Swapping with `r`:** The element at `r` might be a `0`, `1`, or `2`. If it's a `0` or `1`, we need to process it. By not incrementing `i`, we ensure it gets checked in the next iteration.

6. **Time Complexity:** O(N) - each element is visited at most once.
7. **Space Complexity:** O(1) - only three pointers are used.

This problem demonstrates **three-way partitioning**, which is a natural extension of two-way partitioning. The pattern of maintaining multiple boundaries (left, right, and a scanning pointer) is powerful for sorting and partitioning problems.

**Code:**

```python
class Solution:
    def sortColors(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        i, l, r = 0, 0, len(nums) - 1
        while i <= r:
            if nums[i] == 2:
                nums[i], nums[r] = nums[r], nums[i]
                r -= 1
            elif nums[i] == 0:
                nums[i], nums[l] = nums[l], nums[i]
                l += 1
                i += 1
            else:
                i += 1
```

## Problem: 633. Sum of Square Numbers

Given a non-negative integer `c`, decide whether there're two integers `a` and `b` such that `a² + b² = c`.

### Example 1

```
Input: c = 5
Output: true
Explanation: 1 * 1 + 2 * 2 = 5
```

### Example 2

```
Input: c = 3
Output: false
```

### Constraints

- `0 <= c <= 2^31 - 1`

### Solution

**Logic:**

This problem combines the two-pointer technique from **Problem 167 (Two Sum II)** with **mathematical bounds** and **square calculations**. It demonstrates how to apply two pointers when working with mathematical properties rather than array indices.

1. **Building on Problem 167:** Like Problem 167, we're looking for two numbers that sum to a target. However, instead of an array, we're working with the mathematical constraint that `a² + b² = c`, where `a` and `b` are non-negative integers.

2. **Establishing Bounds:**

   - Since `a` and `b` are non-negative and `a² + b² = c`, we know that `0 ≤ a ≤ √c` and `0 ≤ b ≤ √c`.
   - We can set `l = 0` and `r = floor(√c)` as our initial bounds.

3. **Two-Pointer Strategy:**

   - Start with `l = 0` (or `1` in the code) and `r = floor(√c)`.
   - Calculate `sq_sum = l² + r²`.
   - If `sq_sum == c`, we've found our answer.
   - If `sq_sum > c`, we need a smaller sum. Decrement `r` to reduce `r²`.
   - If `sq_sum < c`, we need a larger sum. Increment `l` to increase `l²`.

4. **Edge Cases:** The code handles special cases:

   - If `c` is a perfect square itself (`c_root == r`), then `0² + (√c)² = c`, so return `True`.
   - If `c/2` is a perfect square, then `(√(c/2))² + (√(c/2))² = c`, so return `True`.

5. **Key Insight:** This problem shows that two pointers can work with **mathematical sequences** (squares of integers) just as effectively as with arrays. The sorted property of squares (0², 1², 2², ...) allows us to use the same greedy two-pointer strategy.

6. **Time Complexity:** O(√c) - we iterate at most √c times.
7. **Space Complexity:** O(1) - only a few variables are used.

This problem demonstrates how the two-pointer technique can be applied to **mathematical problems** where we're searching for values that satisfy an equation, not just array elements. This concept will be extended in Phase 3 problems that combine multiple techniques.

**Code:**

```python
import math

class Solution:
    def judgeSquareSum(self, c: int) -> bool:
        c_root = math.sqrt(c)
        l, r = 1, math.floor(c_root)
        half_root = math.sqrt(c/2)
        if c_root == r or half_root == math.floor(half_root):
            return True

        while l < r:
            sq_sum = l ** 2 + r ** 2
            if sq_sum == c:
                return True
            elif sq_sum > c:
                r -= 1
            else:
                l += 1
        return False
```
