# Pattern: Sliding Window - Phase 3

## Problem: 904. Fruit into Baskets

You are visiting a farm that has a single row of fruit trees arranged from left to right. The trees are represented by an integer array `fruits` where `fruits[i]` is the type of fruit the `ith` tree produces.

You want to collect as much fruit as possible. However, the owner has some strict rules that you must follow:

- You only have two baskets, and each basket can only hold a single type of fruit. There is no limit on the amount of fruit each basket can hold.
- Starting from any tree of your choice, you must pick exactly one fruit from every tree (including the start tree) while moving to the right. The picked fruits must fit in one of your baskets.
- Once you reach a tree with fruit that cannot fit in your baskets, you must stop.

Given the integer array `fruits`, return the maximum number of fruits you can pick.

### Example 1

```
Input: fruits = [1,2,1]
Output: 3
Explanation: We can pick from all 3 trees.
```

### Example 2

```
Input: fruits = [0,1,2,2]
Output: 3
Explanation: We can pick from trees [1,2,2].
If we had started at the first tree, we would only pick from trees [0,1].
```

### Example 3

```
Input: fruits = [1,2,3,2,2]
Output: 4
Explanation: We can pick from trees [2,3,2,2].
If we had started at the first tree, we would only pick from trees [1,2].
```

### Constraints

- `1 <= fruits.length <= 10^5`
- `0 <= fruits[i] < fruits.length`

### Solution

**Logic:**

This problem applies variable-size sliding window to **distinct element counting**. We need to find the longest subarray with at most 2 distinct fruit types (since we only have 2 baskets).

1. **Variable-Size Window Setup:** Start with left pointer `l = 0`, a frequency map `check` to track fruit types and their counts in the current window, and result `res = 0`.

2. **The Expanding Strategy:** Move the right pointer `r` through the array:

   - Add `fruits[r]` to the frequency map: `check[fruits[r]] += 1`
   - Check if we've exceeded the constraint

3. **The Constraint Check:** We can only have at most 2 distinct fruit types. Check if `len(check) > 2`.

4. **The Shrinking Strategy:** When `len(check) > 2`:

   - Remove the leftmost fruit: `check[fruits[l]] -= 1`
   - If its count reaches 0, remove it from the map: `check.pop(fruits[l])`
   - Move the left pointer: `l += 1`
   - Continue until we have at most 2 distinct types

5. **Tracking the Maximum:** After ensuring the constraint is satisfied, update the maximum window size: `res = max(res, r-l+1)`.

6. **Why This Works:** By shrinking from the left until we have at most 2 distinct types, we maintain valid windows. We track the maximum window size across all valid windows ending at each position `r`.

7. **Key Insight:** The frequency map efficiently tracks distinct types: when a count drops to 0 and we remove it, `len(check)` decreases, signaling we've eliminated a distinct type.

8. **Optimization:** The alternative solution uses a `distinct` counter to avoid calling `len(check)`, which can be expensive. This optimizes time complexity while slightly increasing space complexity.

9. **Time Complexity:** O(N) - each fruit is added once and removed at most once. In the first solution, `len(check)` is O(1) for small maps.
10. **Space Complexity:** O(1) - the map contains at most 3 entries (when shrinking).

This problem demonstrates how variable-size sliding windows work with **distinct element constraints** using frequency maps.

**Code:**

```python
class Solution:
    def totalFruit(self, fruits: List[int]) -> int:
        check = {}
        l, res = 0, 0
        for r in range(len(fruits)):
            check[fruits[r]] = check.get(fruits[r], 0) + 1
            if len(check) > 2:
                check[fruits[l]] -= 1
                if check[fruits[l]] == 0:
                    check.pop(fruits[l])
                l += 1
            res = max(res, r - l + 1)
        return res
```

OR the below solution which optimizes time complexity but increases space complexity

```python
class Solution:
    def totalFruit(self, fruits: List[int]) -> int:
        check = {}
        l, res, distinct = 0, 0, 0
        for r in range(len(fruits)):
            check[fruits[r]] = check.get(fruits[r], 0) + 1
            if check[fruits[r]] == 1:
                distinct += 1
            if distinct > 2:
                check[fruits[l]] -= 1
                if check[fruits[l]] == 0:
                    check.pop(fruits[l])
                    distinct -= 1
                l += 1
            if distinct <= 2:
                res = max(res, r - l + 1)
        return res
```

## Problem: 713. Subarray Product Less Than K

Given an array of integers `nums` and an integer `k`, return the number of contiguous subarrays where the product of all the elements in the subarray is strictly less than `k`.

### Example 1

```
Input: nums = [10,5,2,6], k = 100
Output: 8
Explanation: The 8 subarrays that have product less than 100 are:
[10], [5], [2], [6], [10, 5], [5, 2], [2, 6], [5, 2, 6]
Note that [10, 5, 2] is not included as the product of 100 is not strictly less than k.
```

### Example 2

```
Input: nums = [1,2,3], k = 0
Output: 0
```

### Constraints

- `1 <= nums.length <= 3 * 10^4`
- `1 <= nums[i] <= 1000`
- `0 <= k <= 10^6`

### Solution

**Logic:**

This problem applies variable-size sliding window to **product constraints**. Unlike sum constraints, products grow multiplicatively, making this more challenging. The key insight is counting all valid subarrays ending at each position.

1. **Variable-Size Window Setup:** Start with left pointer `l = 0`, product `prod = 1`, and result `res = 0`.

2. **The Expanding Strategy:** Move the right pointer `r` through the array:

   - Multiply the product by `nums[r]`: `prod *= nums[r]`
   - Check if the product violates the constraint

3. **The Constraint Check:** We need `prod < k`. If `prod >= k`, we must shrink.

4. **The Shrinking Strategy:** When `prod >= k`:

   - Divide out the leftmost element: `prod /= nums[l]`
   - Move the left pointer: `l += 1`
   - Continue shrinking while `prod >= k and l <= r` (the `l <= r` check prevents negative window size)

5. **Counting Valid Subarrays:** After ensuring `prod < k`, count all valid subarrays ending at position `r`:

   - For a window `[l, r]`, all subarrays `[i, r]` where `l <= i <= r` have product less than `k`
   - There are exactly `r-l+1` such subarrays
   - Add this to the result: `res += (r-l+1)`

6. **Why This Works:** By maintaining the largest valid window ending at `r`, we can count all valid subarrays ending at `r` in one step. This avoids double-counting and ensures we count each valid subarray exactly once.

7. **Key Insight:** The critical realization is that if a window `[l, r]` is valid (product < k), then all subarrays `[i, r]` for `l <= i <= r` are also valid. This is because products are positive (nums[i] >= 1), so removing elements from the left only decreases the product.

8. **Edge Case:** If `k <= 1`, no subarray can have product less than `k` (since `nums[i] >= 1`), so the answer is 0. However, the code handles this naturally by shrinking until the window becomes empty.

9. **Time Complexity:** O(N) - each element is multiplied once and divided out at most once.
10. **Space Complexity:** O(1) - only a few variables are used.

This problem demonstrates how variable-size sliding windows work with **multiplicative constraints** and shows the technique of **counting all valid subarrays** efficiently.

**Code:**

```python
class Solution:
    def numSubarrayProductLessThanK(self, nums: List[int], k: int) -> int:
        l, res, prod = 0, 0, 1
        for r in range(len(nums)):
            prod *= nums[r]
            while prod >= k and l <= r:
                prod /= nums[l]
                l += 1
            res += (r-l+1)

        return res
```

## Problem: 76. Minimum Window Substring (Hard)

Given two strings `s` and `t` of lengths `m` and `n` respectively, return the minimum window substring of `s` such that every character in `t` (including duplicates) is included in the window. If there is no such substring, return the empty string `""`.

The testcases will be generated such that the answer is unique.

### Example 1

```
Input: s = "ADOBECODEBANC", t = "ABC"
Output: "BANC"
Explanation: The minimum window substring "BANC" includes 'A', 'B', and 'C' from string t.
```

### Example 2

```
Input: s = "a", t = "a"
Output: "a"
Explanation: The entire string s is the minimum window.
```

### Example 3

```
Input: s = "a", t = "aa"
Output: ""
Explanation: Both 'a's from t must be included in the window.
Since the largest window of s only has one 'a', return empty string.
```

### Constraints

- `m == s.length`
- `n == t.length`
- `1 <= m, n <= 10^5`
- `s` and `t` consist of uppercase and lowercase English letters.

**Follow up:** Could you find an algorithm that runs in `O(m + n)` time?

### Solution

**Logic:**

This is one of the most sophisticated sliding window problems. It combines variable-size windows with **frequency map matching** and **validity tracking**. We need to find the minimum window in `s` that contains all characters from `t` (with their required frequencies).

1. **Frequency Map Setup:** Build two frequency maps:

   - `need`: Character frequency map for string `t` (what we need)
   - `window`: Character frequency map for the current window in `s` (what we have)

2. **Validity Tracking:** Use a `valid` counter to track how many characters in `need` have been satisfied:

   - When `window[c] == need[c]` for a character `c`, increment `valid`
   - When `valid == len(need)`, all required characters are satisfied

3. **Variable-Size Window Setup:** Start with `left = right = 0`, `valid = 0`, and track the minimum window with `start` and `length`.

4. **The Expanding Strategy (Right Pointer):** Move the right pointer through `s`:

   - Add `s[right]` to the window
   - If `s[right]` is in `need`, increment `window[s[right]]`
   - If `window[s[right]] == need[s[right]]`, increment `valid` (one character requirement satisfied)

5. **The Shrinking Strategy (Left Pointer):** When `valid == len(need)`, we have a valid window. Now shrink from the left:

   - Update the minimum window if current window is smaller: `if right - left < length`
   - Remove `s[left]` from the window
   - If `s[left]` is in `need`:
     - If `window[s[left]] == need[s[left]]`, decrement `valid` (this character is no longer satisfied)
     - Decrement `window[s[left]]`
   - Move the left pointer: `left += 1`
   - Continue shrinking while `valid == len(need)` (while the window is still valid)

6. **Why This Works:** By expanding until we have a valid window, then shrinking while maintaining validity, we ensure we've found the minimum valid window ending at each position. The `valid` counter efficiently tracks whether all character requirements are met without comparing entire maps.

7. **Key Insight:** The `valid` counter is crucial - it avoids expensive map comparisons at each step. We only need to check `valid == len(need)` instead of comparing the entire `window` and `need` maps.

8. **Time Complexity:** O(M + N) where M is length of `s` and N is length of `t`. Each character in `s` is added once (right pointer) and removed at most once (left pointer). The `valid` counter updates are O(1).

9. **Space Complexity:** O(M + N) - the frequency maps contain at most all distinct characters in `s` and `t`. In practice, if we only track characters in `need`, it's O(min(M, 52)) since there are at most 52 distinct letters.

This problem represents the synthesis of sliding window techniques: **variable-size windows, frequency maps, and validity tracking** for finding optimal windows.

**Code:**

```python
class Solution:
    def minWindow(self, s: str, t: str) -> str:
        if len(t) > len(s):
            return ""

        need = {}
        window = {}

        for char in t:
            need[char] = need.get(char, 0) + 1

        left, right = 0, 0
        valid = 0
        start, length = 0, float('inf')

        while right < len(s):
            c = s[right]
            right += 1

            if c in need:
                window[c] = window.get(c, 0) + 1
                if window[c] == need[c]:
                    valid += 1

            while valid == len(need):
                if right - left < length:
                    start = left
                    length = right - left

                d = s[left]
                left += 1

                if d in need:
                    if window[d] == need[d]:
                        valid -= 1
                    window[d] -= 1

        return "" if length == float('inf') else s[start:start+length]
```

## Problem: 239. Sliding Window Maximum (Hard)

You are given an array of integers `nums`, there is a sliding window of size `k` which is moving from the very left of the array to the very right. You can only see the `k` numbers in the window. Each time the sliding window moves right by one position.

Return the maximum sliding window.

### Example 1

```
Input: nums = [1,3,-1,-3,5,3,6,7], k = 3
Output: [3,3,5,5,6,7]
Explanation:
Window position                Max
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```

### Example 2

```
Input: nums = [1], k = 1
Output: [1]
```

### Constraints

- `1 <= nums.length <= 10^5`
- `-10^4 <= nums[i] <= 10^4`
- `1 <= k <= nums.length`

### Solution

**Logic:**

This problem introduces the **monotonic deque** technique. Unlike previous sliding window problems, this uses a fixed-size window but requires maintaining the maximum efficiently. A naive approach would be O(N\*K), but using a deque we achieve O(N).

1. **Monotonic Deque Setup:** Use a deque (double-ended queue) to store **indices** of elements in decreasing order of their values. The front of the deque always contains the index of the maximum element in the current window.

2. **Why Store Indices:** We store indices rather than values because:

   - We need to remove elements that fall outside the window
   - We need to access the array elements when adding to results

3. **The Sliding Strategy:** For each position `i` in the array:

   - **Remove indices outside the window:** Remove indices from the front where `dq[0] < i - k + 1` (these are outside the current window)
   - **Remove indices with smaller values:** Remove indices from the back where `nums[dq[-1]] < nums[i]`:
     - These elements can never be the maximum (since `nums[i]` is larger and appears later)
     - This maintains the deque in decreasing order
   - **Add current index:** Append `i` to the deque
   - **Record maximum:** If we've seen at least `k` elements (`i >= k - 1`), the front of the deque contains the maximum index, so add `nums[dq[0]]` to results

4. **Why This Works:** The deque maintains indices in decreasing order of their values. When we add a new element:

   - All elements smaller than it (and appearing earlier) are removed - they can't be maximums
   - The front always has the index of the maximum in the current window
   - Removing indices outside the window keeps it valid

5. **Key Insight:** This is a **monotonic decreasing deque**. Elements are stored from front (maximum) to back (minimum) by value, and in increasing order by index. When a larger element comes in, it eliminates all smaller elements that appeared before it.

6. **Time Complexity:** O(N) - each element is added to the deque once and removed at most once. Even though we have nested while loops, each element is processed at most twice (added once, removed once).
7. **Space Complexity:** O(K) - the deque stores at most `k` indices (one for each element in the window).

This problem demonstrates the **monotonic deque pattern**, which is essential for efficiently maintaining extremal values (maximums/minimums) in sliding windows.

**Code:**

```python
from collections import deque

class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        dq = deque()
        result = []

        for i in range(len(nums)):
            while dq and dq[0] < i - k + 1:
                dq.popleft()

            while dq and nums[dq[-1]] < nums[i]:
                dq.pop()

            dq.append(i)

            if i >= k - 1:
                result.append(nums[dq[0]])

        return result
```
