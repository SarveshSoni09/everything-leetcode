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

This is the foundational two-pointer problem that demonstrates the core pattern. The key insight is that because the array is **sorted**, we can use two pointers starting from opposite ends to efficiently find the target sum.

1. **Initial Setup:** Place one pointer `i` at the start (index 0) and another pointer `j` at the end (index `len(numbers)-1`).

2. **The Two-Pointer Strategy:** At each step, we compare `numbers[i] + numbers[j]` with `target`:

   - If the sum equals `target`, we've found our answer.
   - If the sum is **less than** `target`, we need a larger sum. Since the array is sorted, moving `i` to the right (incrementing) will give us a larger value.
   - If the sum is **greater than** `target`, we need a smaller sum. Moving `j` to the left (decrementing) will give us a smaller value.

3. **Why This Works:** The sorted property guarantees that:

   - All elements to the left of `i` are smaller than `numbers[i]`
   - All elements to the right of `j` are larger than `numbers[j]`
   - This allows us to make greedy decisions about which pointer to move, eliminating half the search space at each step.

4. **Time Complexity:** O(N) - each element is visited at most once.
5. **Space Complexity:** O(1) - only two pointers are used.

This problem establishes the fundamental two-pointer technique: **start from opposite ends and move pointers based on comparison with a target value**.

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

This problem introduces the two-pointer technique for **string validation**. The core idea is to compare characters from both ends, moving inward until the pointers meet.

1. **Preprocessing:** First, we normalize the string by converting to lowercase and removing non-alphanumeric characters. This creates a clean string that only contains the characters we care about.

2. **Two-Pointer Approach:**

   - Place `i` at the start (index 0) and `j` at the end (index `len(s)-1`).
   - While `i < j`, compare `s[i]` and `s[j]`.
   - If they don't match, return `False` immediately.
   - If they match, move both pointers inward: `i += 1` and `j -= 1`.

3. **Termination:** The loop continues until `i >= j`, meaning we've checked all pairs. If we exit the loop without finding a mismatch, the string is a palindrome.

4. **Key Insight:** This problem demonstrates that two pointers can work on strings just as effectively as arrays. The pattern of "compare from both ends and move inward" is a fundamental two-pointer technique.

5. **Time Complexity:** O(N) - we process each character at most once.
6. **Space Complexity:** O(N) - we create a new string with only alphanumeric characters.

This problem builds on the two-pointer concept from Problem 167, but applies it to **character-by-character comparison** rather than sum calculation.

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

## Problem: 344. Reverse String

Write a function that reverses a string. The input string is given as an array of characters `s`.

You must do this by modifying the input array in-place with `O(1)` extra memory.

### Example 1

```
Input: s = ["h","e","l","l","o"]
Output: ["o","l","l","e","h"]
```

### Example 2

```
Input: s = ["H","a","n","n","a","h"]
Output: ["h","a","n","n","a","H"]
```

### Constraints

- `1 <= s.length <= 10^5`
- `s[i]` is a printable ascii character.

### Solution

**Logic:**

This problem demonstrates the two-pointer technique for **in-place array manipulation**. It's one of the simplest applications of swapping elements from opposite ends.

1. **Two-Pointer Setup:** Place `l` at the start (index 0) and `r` at the end (index `len(s)-1`).

2. **Swapping Strategy:**

   - While `l < r`, swap `s[l]` and `s[r]`.
   - Move `l` right: `l += 1`
   - Move `r` left: `r -= 1`

3. **Why This Works:** By swapping elements from opposite ends and moving inward, we effectively reverse the entire array. The first element becomes the last, the second becomes the second-to-last, and so on.

4. **Termination:** When `l >= r`, we've swapped all necessary pairs. If the array has an odd length, the middle element stays in place (which is correct).

5. **Key Insight:** This problem shows that two pointers can be used for **element swapping**, not just comparison. The pattern of "swap and move inward" is fundamental to many in-place algorithms.

6. **Time Complexity:** O(N) - we swap N/2 pairs.
7. **Space Complexity:** O(1) - only two pointers are used, no extra space.

This problem builds on the two-pointer concept by introducing **element swapping** as a core operation, which will be used in more complex problems later.

**Code:**

```python
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        l, r = 0, len(s)-1
        while l<r:
            s[l], s[r] = s[r], s[l]
            l += 1
            r -= 1
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

This problem introduces a key insight: when dealing with **squares** of a sorted array containing negative numbers, the largest squares come from the **extremes** (most negative or most positive), not necessarily from the ends in order.

1. **The Challenge:** After squaring, `[-4, -1, 0, 3, 10]` becomes `[16, 1, 0, 9, 100]`. The largest square (16) comes from the most negative number (-4), not from the end of the original array.

2. **Two-Pointer Strategy:**

   - Start with `i` at the beginning and `j` at the end.
   - Compare the **absolute values** of `nums[i]` and `nums[j]`.
   - The one with the larger absolute value will produce the larger square.
   - Place the larger square at the **end** of the result array (since we're building it in reverse order).
   - Move the pointer that contributed the larger square.

3. **Building the Result:**

   - We build the result array from right to left (largest to smallest).
   - After processing all elements, we reverse the result array to get the correct order.

4. **Why This Works:** The key insight is that in a sorted array with negatives, the largest squares are at the extremes. By comparing absolute values and always taking the larger one, we can build the sorted squares array in one pass.

5. **Time Complexity:** O(N) - each element is processed exactly once.
6. **Space Complexity:** O(N) - for the result array.

This problem demonstrates that two pointers can be used to **merge or combine** elements from opposite ends, building a result array in a specific order. This concept will be extended in more complex problems.

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

## Problem: 345. Reverse Vowels of a String

Given a string `s`, reverse only all the vowels in the string and return it.

The vowels are `'a'`, `'e'`, `'i'`, `'o'`, and `'u'`, and they can appear in both lower and upper cases, more than once.

### Example 1

```
Input: s = "IceCreAm"
Output: "AceCreIm"
Explanation:
The vowels in s are ['I', 'e', 'e', 'A']. On reversing the vowels, s becomes "AceCreIm".
```

### Example 2

```
Input: s = "leetcode"
Output: "leotcede"
```

### Constraints

- `1 <= s.length <= 3 * 10^5`
- `s` consist of printable ASCII characters.

### Solution

**Logic:**

This problem combines the **swapping** technique from Problem 344 with **conditional pointer movement** based on character properties. It introduces the concept of "skip non-target elements" while maintaining the two-pointer approach.

1. **The Challenge:** We need to reverse only vowels while keeping consonants in their original positions. This requires finding vowels from both ends and swapping them.

2. **Two-Pointer with Conditional Movement:**

   - Start with `l` at the beginning and `r` at the end.
   - **Skip non-vowels:** While `s[l]` is not a vowel and `l < r`, increment `l`. Similarly, while `s[r]` is not a vowel and `l < r`, decrement `r`.
   - Once both pointers are on vowels, swap them.
   - Move both pointers inward: `l += 1` and `r -= 1`.

3. **Key Insight:** This problem introduces the pattern of **conditional pointer advancement**. Instead of always moving both pointers, we move them only when they're pointing to the elements we care about (vowels in this case).

4. **Why This Works:** By skipping non-vowels, we ensure that:

   - Consonants stay in their original positions
   - Only vowels get swapped with each other
   - The relative order of vowels is reversed

5. **Time Complexity:** O(N) - each character is visited at most once.
6. **Space Complexity:** O(N) - we convert the string to a list for in-place modification.

This problem builds on Problem 344 (swapping) and Problem 125 (character checking), combining them to create a **selective swapping** algorithm. This pattern of "find target elements from both ends and swap" will be used in more complex problems like Problem 905.

**Code:**

```python
class Solution:
    def reverseVowels(self, s: str) -> str:
        s = list(s)
        l, r = 0, len(s) - 1
        vowels = "aeiouAEIOU"
        while l < r:
            while s[l] not in vowels and l < r:
                l += 1
            while s[r] not in vowels and l < r:
                r -= 1
            s[l], s[r] = s[r], s[l]
            l += 1
            r -= 1
        return "".join(s)
```
