# Pattern: Sliding Window - Phase 2

## Problem: 209. Minimum Size Subarray Sum

Given an array of positive integers `nums` and a positive integer `target`, return the minimal length of a subarray whose sum is greater than or equal to `target`. If there is no such subarray, return `0` instead.

### Example 1

```
Input: target = 7, nums = [2,3,1,2,4,3]
Output: 2
Explanation: The subarray [4,3] has the minimal length under the problem constraint.
```

### Example 2

```
Input: target = 4, nums = [1,4,4]
Output: 1
```

### Example 3

```
Input: target = 11, nums = [1,1,1,1,1,1,1,1]
Output: 0
```

### Constraints

- `1 <= target <= 10^9`
- `1 <= nums.length <= 10^5`
- `1 <= nums[i] <= 10^4`

**Follow up:** If you have figured out the O(n) solution, try coding another solution of which the time complexity is O(n log(n)).

### Solution

**Logic:**

**Code:**

```python
class Solution:
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        l, curr_sum = 0, 0
        res = float('inf')

        for r in range(len(nums)):
            curr_sum += nums[r]

            while curr_sum >= target:
                if r-l+1 < res:
                    res = r-l+1
                curr_sum -= nums[l]
                l += 1
        return res if res != float('inf') else 0
```

## Problem: 3. Longest Substring Without Repeating Characters

Given a string `s`, find the length of the longest substring without duplicate characters.

### Example 1

```
Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3. Note that "bca" and "cab" are also correct answers.
```

### Example 2

```
Input: s = "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
```

### Example 3

```
Input: s = "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.
```

Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.

### Constraints

- `0 <= s.length <= 5 * 10^4`
- `s` consists of English letters, digits, symbols and spaces.

### Solution

**Logic:**

**Code:**

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        l, res = 0, 0
        check = set()

        for r in range(len(s)):
            while s[r] in check:
                check.remove(s[l])
                l += 1
            check.add(s[r])
            if r-l+1 > res:
                res = r-l+1
        return res
```

## Problem: 1004. Maximum Consecutive Ones III

Given a binary array `nums` and an integer `k`, return the maximum number of consecutive `1`'s in the array if you can flip at most `k` `0`'s.

### Example 1

```
Input: nums = [1,1,1,0,0,0,1,1,1,1,0], k = 2
Output: 6
Explanation: [1,1,1,0,0,1,1,1,1,1,1]
Bolded numbers were flipped from 0 to 1. The longest subarray is underlined.
```

### Example 2

```
Input: nums = [0,0,1,1,0,0,1,1,1,0,1,1,0,0,0,1,1,1,1], k = 3
Output: 10
Explanation: [0,0,1,1,1,1,1,1,1,1,1,1,0,0,0,1,1,1,1]
Bolded numbers were flipped from 0 to 1. The longest subarray is underlined.
```

### Constraints

- `1 <= nums.length <= 10^5`
- `nums[i]` is either `0` or `1`.
- `0 <= k <= nums.length`

### Solution

**Logic:**

**Code:**

```python
class Solution:
    def longestOnes(self, nums: List[int], k: int) -> int:
        zeros, l, res = 0, 0, 0

        for r in range(len(nums)):
            if nums[r] == 0:
                if zeros == k:
                    while nums[l] != 0:
                        l += 1
                    l += 1
                else:
                    zeros += 1
            res = max(res, r-l+1)
        return res
```

## Problem: 424. Longest Repeating Character Replacement

You are given a string `s` and an integer `k`. You can choose any character of the string and change it to any other uppercase English character. You can perform this operation at most `k` times.

Return the length of the longest substring containing the same letter you can get after performing the above operations.

### Example 1

```
Input: s = "ABAB", k = 2
Output: 4
Explanation: Replace the two 'A's with two 'B's or vice versa.
```

### Example 2

```
Input: s = "AABABBA", k = 1
Output: 4
Explanation: Replace the one 'A' in the middle with 'B' and form "AABBBBA".
The substring "BBBB" has the longest repeating letters, which is 4.
There may exists other ways to achieve this answer too.
```

### Constraints

- `1 <= s.length <= 10^5`
- `s` consists of only uppercase English letters.
- `0 <= k <= s.length`

### Solution

**Logic:**

**Code:**

```python
class Solution:
    def characterReplacement(self, s: str, k: int) -> int:
        count = {}
        l, res = 0, 0
        for r in range(len(s)):
            count[s[r]] = 1 + count.get(s[r], 0)
            while (r - l + 1) - max(count.values()) > k:
                count[s[l]] -= 1
                l += 1
            res = max(res, r - l + 1)
        return res
```

