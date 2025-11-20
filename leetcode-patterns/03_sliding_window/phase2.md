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

This problem introduces the **variable-size sliding window** pattern. Unlike fixed-size windows (Phase 1), here the window size changes dynamically based on a condition. We expand the window until a condition is met, then shrink it while maintaining the condition.

1. **Variable-Size Window Setup:** Start with left pointer `l = 0`, current sum `curr_sum = 0`, and result `res = float('inf')`.

2. **The Expanding Strategy:** Move the right pointer `r` through the array:

   - Add the element at `r` to the current sum: `curr_sum += nums[r]`
   - If `curr_sum >= target`, we've found a valid window

3. **The Shrinking Strategy:** Once `curr_sum >= target`, we shrink from the left:

   - Update the minimum window size: `res = min(res, r-l+1)`
   - Remove the leftmost element: `curr_sum -= nums[l]`
   - Move the left pointer: `l += 1`
   - Continue shrinking while `curr_sum >= target` (using a `while` loop)

4. **Why This Works:** By shrinking from the left while maintaining `curr_sum >= target`, we find the minimum valid window ending at each position `r`. This ensures we don't miss any optimal solutions.

5. **Key Insight:** The `while` loop for shrinking is crucial. It allows us to keep shrinking until the window becomes invalid, ensuring we've found the smallest valid window ending at each `r`.

6. **Time Complexity:** O(N) - each element is added once (right pointer) and removed at most once (left pointer).
7. **Space Complexity:** O(1) - only a few variables are used.

This problem establishes the fundamental variable-size sliding window pattern: **expand until condition is met, then shrink while maintaining the condition**.

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

This problem applies the variable-size sliding window pattern to **character uniqueness**. We use a set to track characters in the current window and shrink when a duplicate is encountered.

1. **Variable-Size Window Setup:** Start with left pointer `l = 0`, a set `check` to track characters in the current window, and result `res = 0`.

2. **The Expanding Strategy:** Move the right pointer `r` through the string:

   - If `s[r]` is not in `check`, add it and expand the window
   - If `s[r]` is already in `check`, we have a duplicate

3. **The Shrinking Strategy:** When a duplicate is found (`s[r] in check`):

   - Remove characters from the left using a `while` loop until the duplicate is eliminated
   - Remove `s[l]` from `check`: `check.remove(s[l])`
   - Move the left pointer: `l += 1`
   - Continue until `s[r]` is no longer in `check`

4. **Tracking the Maximum:** After ensuring no duplicates, add `s[r]` to `check` and update the maximum window size: `res = max(res, r-l+1)`.

5. **Why This Works:** By shrinking from the left until the duplicate is removed, we ensure the window always contains unique characters. This guarantees we find the longest valid window ending at each position `r`.

6. **Key Insight:** Using a `while` loop for shrinking is essential. We must remove all characters from the left until the duplicate `s[r]` is eliminated, not just one character.

7. **Time Complexity:** O(N) - each character is added once and removed at most once.
8. **Space Complexity:** O(min(N, M)) where M is the size of the charset - the set stores at most all distinct characters in the string.

This problem demonstrates how variable-size sliding windows work with **set-based uniqueness checking**.

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

This problem applies variable-size sliding window to **constraint satisfaction**. We're allowed at most `k` zeros in our window, so we expand until we exceed this limit, then shrink appropriately.

1. **Variable-Size Window Setup:** Start with left pointer `l = 0`, a counter `zeros` to track the number of zeros in the current window, and result `res = 0`.

2. **The Expanding Strategy:** Move the right pointer `r` through the array:

   - If `nums[r] == 1`, it doesn't affect our constraint (we can always include 1s)
   - If `nums[r] == 0`, we need to check if we can still include it

3. **Handling Zeros:** When we encounter a zero at position `r`:

   - If `zeros < k`: We can include this zero (increment `zeros`)
   - If `zeros == k`: We've reached our limit, must shrink before including

4. **The Shrinking Strategy:** When `zeros == k` and we encounter another zero:

   - Move `l` forward until we remove one zero: `while nums[l] != 0: l += 1`
   - Then move `l` one more position to remove that zero: `l += 1`
   - Now we can include the new zero: `zeros` remains `k`

5. **Tracking the Maximum:** After ensuring the constraint is satisfied, update the maximum window size: `res = max(res, r-l+1)`.

6. **Why This Works:** By tracking the count of zeros and shrinking when we exceed `k`, we maintain a valid window at all times. The window represents the longest subarray with at most `k` zeros ending at position `r`.

7. **Key Insight:** This problem is essentially finding the longest subarray with at most `k` zeros. The flipping operation is conceptual - we're just finding windows where zeros can be flipped to ones.

8. **Time Complexity:** O(N) - each element is visited once, and the left pointer moves forward at most N times.
9. **Space Complexity:** O(1) - only a few variables are used.

This problem demonstrates how variable-size sliding windows work with **constraint counting** (tracking the count of a specific element type).

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

This problem combines variable-size sliding window with **frequency tracking**. The key insight is that we want the longest window where we can change all non-majority characters to match the majority character using at most `k` operations.

1. **Variable-Size Window Setup:** Start with left pointer `l = 0`, a frequency map `count` to track character frequencies in the current window, and result `res = 0`.

2. **The Expanding Strategy:** Move the right pointer `r` through the string:

   - Add `s[r]` to the frequency map: `count[s[r]] += 1`
   - Check if we can still maintain a valid window

3. **The Constraint Check:** For a valid window, the number of characters we need to change is:

   - `window_size - max_frequency = (r-l+1) - max(count.values())`
   - This represents how many characters differ from the most frequent character
   - We can change at most `k` characters, so: `(r-l+1) - max(count.values()) <= k`

4. **The Shrinking Strategy:** When the constraint is violated:

   - Remove the leftmost character: `count[s[l]] -= 1`
   - Move the left pointer: `l += 1`
   - Continue shrinking while `(r-l+1) - max(count.values()) > k`

5. **Tracking the Maximum:** After ensuring the constraint is satisfied, update the maximum window size: `res = max(res, r-l+1)`.

6. **Why This Works:** The window size `(r-l+1)` minus the maximum frequency gives us the minimum number of replacements needed. By shrinking when this exceeds `k`, we maintain valid windows. We track the maximum window size across all valid windows.

7. **Key Insight:** We don't need to know which character to replace - we just need to ensure that after at most `k` replacements, all characters can be the same. This happens when `window_size - max_frequency <= k`.

8. **Time Complexity:** O(N) - each character is added once and removed at most once. However, `max(count.values())` takes O(26) = O(1) time per operation.
9. **Space Complexity:** O(1) - the frequency map contains at most 26 entries (one for each uppercase letter).

This problem demonstrates how variable-size sliding windows work with **frequency maps and constraint optimization** (maximizing window size while satisfying a constraint).

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
