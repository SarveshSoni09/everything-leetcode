# Pattern: Sliding Window - Phase 1

## Problem: 643. Maximum Average Subarray I

You are given an integer array `nums` consisting of `n` elements, and an integer `k`.

Find a contiguous subarray whose length is equal to `k` that has the maximum average value and return this value. Any answer with a calculation error less than 10^-5 will be accepted.

### Example 1

```
Input: nums = [1,12,-5,-6,50,3], k = 4
Output: 12.75000
Explanation: Maximum average is (12 - 5 - 6 + 50) / 4 = 51 / 4 = 12.75
```

### Example 2

```
Input: nums = [5], k = 1
Output: 5.00000
```

### Constraints

- `n == nums.length`
- `1 <= k <= n <= 10^5`
- `-10^4 <= nums[i] <= 10^4`

### Solution

**Logic:**

This is the foundational fixed-size sliding window problem. The key insight is that we need to find the maximum average among all subarrays of fixed length `k`, which is equivalent to finding the maximum sum (since average = sum/k).

1. **Fixed-Size Window Setup:** Initialize the first window by calculating the sum of the first `k` elements. This becomes both our current sum and initial maximum sum.

2. **The Sliding Strategy:** Slide the window one position to the right:

   - Remove the leftmost element: `curr_sum -= nums[i]`
   - Add the new rightmost element: `curr_sum += nums[j]`
   - Update the maximum sum: `max_sum = max(max_sum, curr_sum)`
   - Move the left pointer: `i += 1`

3. **Why This Works:** By sliding the window instead of recalculating the sum from scratch for each position, we achieve O(n) time complexity instead of O(n\*k). We only need to subtract one element and add one element per step.

4. **Key Insight:** Since all windows have the same size `k`, maximizing the sum automatically maximizes the average. We calculate the average only at the end by dividing `max_sum` by `k`.

5. **Time Complexity:** O(N) - we visit each element once.
6. **Space Complexity:** O(1) - only a few variables are used.

This problem establishes the fundamental fixed-size sliding window pattern: **calculate the first window, then slide by removing the leftmost element and adding the rightmost element**.

**Code:**

```python
class Solution:
    def findMaxAverage(self, nums: List[int], k: int) -> float:
        i = 0
        curr_sum, max_sum = sum(nums[:k]), sum(nums[:k])
        for j in range(k, len(nums)):
            curr_sum = curr_sum - nums[i] + nums[j]
            max_sum = max(max_sum, curr_sum)
            i += 1

        return max_sum/k
```

## Problem: 1876. Substrings of Size Three with Distinct Characters

A string is good if there are no repeated characters.

Given a string `s`​​​​​, return the number of good substrings of length three in `s​​​​​​`.

Note that if there are multiple occurrences of the same substring, every occurrence should be counted.

A substring is a contiguous sequence of characters in a string.

### Example 1

```
Input: s = "xyzzaz"
Output: 1
Explanation: There are 4 substrings of size 3: "xyz", "yzz", "zza", and "zaz".
The only good substring of length 3 is "xyz".
```

### Example 2

```
Input: s = "aababcabc"
Output: 4
Explanation: There are 7 substrings of size 3: "aab", "aba", "bab", "abc", "bca", "cab", and "abc".
The good substrings are "abc", "bca", "cab", and "abc".
```

### Constraints

- `1 <= s.length <= 100`
- `s`​​​​​ consists of lowercase English letters.

### Solution

**Logic:**

This problem demonstrates a fixed-size sliding window with a very small window size (3). Because the window is so small, we can check for distinct characters directly without using additional data structures.

1. **Fixed-Size Window Setup:** Use three variables `a`, `b`, and `c` to track the three characters in the current window of size 3.

2. **The Sliding Strategy:**

   - Start with the first window: `a = s[0]`, `b = s[1]`
   - For each new position `i` starting from index 2:
     - Set `c = s[i]` (the new rightmost character)
     - Check if all three characters are distinct: `a != b and b != c and c != a`
     - If they are distinct, increment the result counter
     - Slide the window: `a, b = b, c` (move the window one position right)

3. **Why This Works:** For a window of size 3, there are exactly three pairs to check: `(a,b)`, `(b,c)`, and `(a,c)`. If all three pairs are different, all three characters are distinct. This is more efficient than using a set for such a small window.

4. **Key Insight:** When the window size is small and constant, sometimes a direct comparison approach is simpler and more efficient than using hash sets or maps.

5. **Time Complexity:** O(N) - we visit each character once.
6. **Space Complexity:** O(1) - only three character variables are used.

This problem shows that for very small fixed-size windows, **direct comparison can be more efficient than using data structures**.

**Code:**

```python
class Solution:
    def countGoodSubstrings(self, s: str) -> int:
        if len(s) < 3:
            return 0
        a, b = s[0], s[1]
        res = 0
        for i in range(2, len(s)):
            c = s[i]
            if a != b and b!= c and c!= a:
                res += 1
            a, b = b, c
        return res
```

## Problem: 1456. Maximum Number of Vowels in a Substring of Given Length

Given a string `s` and an integer `k`, return the maximum number of vowel letters in any substring of `s` with length `k`.

Vowel letters in English are `'a'`, `'e'`, `'i'`, `'o'`, and `'u'`.

### Example 1

```
Input: s = "abciiidef", k = 3
Output: 3
Explanation: The substring "iii" contains 3 vowel letters.
```

### Example 2

```
Input: s = "aeiou", k = 2
Output: 2
Explanation: Any substring of length 2 contains 2 vowels.
```

### Example 3

```
Input: s = "leetcode", k = 3
Output: 2
Explanation: "lee", "eet" and "ode" contain 2 vowels.
```

### Constraints

- `1 <= s.length <= 10^5`
- `s` consists of lowercase English letters.
- `1 <= k <= s.length`

### Solutions

**Logic:**

This problem builds on Problem 643 by applying the fixed-size sliding window pattern to counting vowels. The approach is similar: maintain a fixed-size window and efficiently update the vowel count as we slide.

1. **Fixed-Size Window Setup:** Count vowels in the first window of length `k`. Store this as both the current count and initial result.

2. **The Sliding Strategy:** For each new position starting from index `k`:

   - If the new character (entering the window) is a vowel: increment the count
   - If the character leaving the window (at position `i-k`) is a vowel: decrement the count
   - Update the maximum count seen so far
   - Early exit optimization: If we find a window with all vowels (`res == k`), we can return immediately since this is the maximum possible

3. **Why This Works:** Instead of recounting vowels for each window from scratch, we maintain a running count and update it incrementally. This is the same optimization pattern from Problem 643.

4. **Key Insight:** The early exit optimization (`if res == k: return res`) is possible because once we find a window with `k` vowels, no other window can have more (since each window has exactly `k` characters).

5. **Time Complexity:** O(N) - we visit each character once, with potential early exit.
6. **Space Complexity:** O(1) - only a few variables and a small vowel set are used.

This problem demonstrates the same fixed-size sliding window pattern as Problem 643, but applied to **character counting and conditional updates**.

**Code:**

```python
class Solution:
    def maxVowels(self, s: str, k: int) -> int:
        vowels = {"a", "e", "i", "o", "u"}
        count = 0
        for i in range(k):
            if s[i] in vowels:
                count += 1
        res = count
        for i in range(k, len(s)):
            if s[i] in vowels:
                count += 1
            if s[i-k] in vowels:
                count -= 1
            if count > res:
                res = count
                if res == k:
                    return res
        return res
```

## Problem: 567. Permutation in String

Given two strings `s1` and `s2`, return `true` if `s2` contains a permutation of `s1`, or `false` otherwise.

In other words, return `true` if one of `s1`'s permutations is the substring of `s2`.

### Example 1

```
Input: s1 = "ab", s2 = "eidbaooo"
Output: true
Explanation: s2 contains one permutation of s1 ("ba").
```

### Example 2

```
Input: s1 = "ab", s2 = "eidboaoo"
Output: false
```

### Constraints

- `1 <= s1.length, s2.length <= 10^4`
- `s1` and `s2` consist of lowercase English letters.

### Solution

**Logic:**

This problem introduces the **frequency map** approach for fixed-size sliding windows. A permutation has the same character frequencies as the original string, so we can use frequency maps to check for matches.

1. **Frequency Map Setup:** Build a frequency map `check` for string `s1` (the target pattern). Also create a matching map `res` initialized to 0 for all characters in `s1`.

2. **Initial Window:** Build the frequency map for the first window of length `len(s1)` in `s2`. If the frequency maps match, return `true` immediately.

3. **The Sliding Strategy:** For each new position starting from index `n`:

   - Add the new character (entering the window): if `s2[i]` is in `res`, increment its count
   - Remove the character leaving the window: if `s2[i-n]` is in `res`, decrement its count
   - Check if frequency maps match: if `check == res`, we found a permutation

4. **Why This Works:** Two strings are permutations of each other if and only if they have identical character frequencies. By maintaining frequency maps for both the target and the current window, we can efficiently check for permutations.

5. **Key Insight:** We only track characters that appear in `s1` in the `res` map. Characters not in `s1` don't affect the permutation check, so we can ignore them.

6. **Time Complexity:** O(N) - we visit each character in `s2` once, and map comparisons are O(1) since there are at most 26 distinct characters.
7. **Space Complexity:** O(1) - the maps contain at most 26 entries.

This problem establishes the **frequency map pattern** for fixed-size windows, which is crucial for anagram and permutation problems.

**Code:**

```python
class Solution:
    def checkInclusion(self, s1: str, s2: str) -> bool:
        if len(s2) < len(s1):
            return False
        check, res = {}, {}
        n = len(s1)
        for s in s1:
            check[s] = check.get(s, 0) + 1
            res[s] = 0

        for i in range(n):
            if s2[i] in res:
                res[s2[i]] = res.get(s2[i], 0) + 1
        if check == res:
            return True
        for i in range(n, len(s2)):
            if s2[i] in res:
                res[s2[i]] += 1
            if s2[i-n] in res:
                res[s2[i-n]] -= 1
            if check == res:
                return True
        return False
```

## Problem: 438. Find All Anagrams in a String

Given two strings `s` and `p`, return an array of all the start indices of `p`'s anagrams in `s`. You may return the answer in any order.

### Example 1

```
Input: s = "cbaebabacd", p = "abc"
Output: [0,6]
Explanation:
The substring with start index = 0 is "cba", which is an anagram of "abc".
The substring with start index = 6 is "bac", which is an anagram of "abc".
```

### Example 2

```
Input: s = "abab", p = "ab"
Output: [0,1,2]
Explanation:
The substring with start index = 0 is "ab", which is an anagram of "ab".
The substring with start index = 1 is "ba", which is an anagram of "ab".
The substring with start index = 2 is "ab", which is an anagram of "ab".
```

### Solution

**Logic:**

This problem is a direct extension of Problem 567. The core approach is identical: use frequency maps with a fixed-size sliding window. The only difference is that instead of returning `true` on the first match, we collect all start indices where anagrams are found.

1. **Building on Problem 567:** The frequency map setup and sliding strategy are the same:

   - Build frequency map `check_p` for string `p`
   - Create matching map `check_s` for tracking the current window in `s`

2. **The Collection Strategy:** When frequency maps match (`check_p == check_s`), instead of returning `true`, we:

   - Add the start index `i-n+1` to the result list
   - Continue sliding to find all possible anagrams

3. **Why This Works:** An anagram is essentially a permutation, so the same frequency map comparison applies. The difference is we're collecting all matches rather than stopping at the first one.

4. **Key Insight:** The start index calculation `i-n+1` comes from: when the right pointer is at index `i`, the window spans from `i-n+1` to `i` (inclusive), which is exactly `n` characters.

5. **Time Complexity:** O(N) - we visit each character in `s` once, and map comparisons are O(1).
6. **Space Complexity:** O(1) - the maps contain at most 26 entries.

This problem demonstrates that the frequency map pattern from Problem 567 can be **extended to find all occurrences** by collecting indices instead of returning early.

**Code:**

```python
class Solution:
    def findAnagrams(self, s: str, p: str) -> List[int]:
        if len(s) < len(p):
            return []
        n = len(p)
        check_p, check_s = {}, {}
        res = []
        for c in p:
            check_p[c] = check_p.get(c, 0) + 1
            check_s[c] = 0
        for i in range(n):
            if s[i] in check_s:
                check_s[s[i]] += 1
        if check_p == check_s:
            res.append(0)
        for i in range(n, len(s)):
            if s[i] in check_s:
                check_s[s[i]] += 1
            if s[i-n] in check_s:
                check_s[s[i-n]] -= 1
            if check_p == check_s:
                res.append(i-n+1)
        return res
```

## Problem: 1343. Number of Sub-arrays of Size K and Average Greater than or Equal to Threshold

Given an array of integers `arr` and two integers `k` and `threshold`, return the number of sub-arrays of size `k` and average greater than or equal to `threshold`.

### Example 1

```
Input: arr = [2,2,2,2,5,5,5,8], k = 3, threshold = 4
Output: 3
Explanation: Sub-arrays [2,5,5],[5,5,5] and [5,5,8] have averages 4, 5 and 6 respectively. All other sub-arrays of size 3 have averages less than 4 (the threshold).
```

### Example 2

```
Input: arr = [11,13,17,23,29,31,7,5,2,3], k = 3, threshold = 5
Output: 6
Explanation: The first 6 sub-arrays of size 3 have averages greater than 5. Note that averages are not integers.
```

### Constraints

- `1 <= arr.length <= 10^5`
- `1 <= arr[i] <= 10^4`
- `1 <= k <= arr.length`
- `0 <= threshold <= 10^4`

### Solution

**Logic:**

This problem combines the fixed-size sliding window pattern from Problem 643 with a threshold comparison. Instead of finding the maximum, we count how many windows meet a certain condition.

1. **The Key Transformation:** Instead of comparing averages directly, we multiply both sides by `k`:

   - `average >= threshold` is equivalent to `sum >= k * threshold`
   - This avoids floating-point arithmetic and simplifies the comparison

2. **Fixed-Size Window Setup:** Calculate the sum of the first window. If `curr_sum >= target` (where `target = k * threshold`), increment the counter.

3. **The Sliding Strategy:** For each new position:

   - Remove the leftmost element: `curr_sum -= arr[i-k]`
   - Add the new rightmost element: `curr_sum += arr[i]`
   - If `curr_sum >= target`, increment the counter

4. **Why This Works:** By using integer comparison instead of floating-point averages, we avoid precision issues and make the code simpler and more efficient. The mathematical equivalence `average >= threshold ⇔ sum >= k*threshold` is valid because all windows have the same size `k`.

5. **Key Insight:** This demonstrates a common optimization: **transform the problem to use integer arithmetic when possible** instead of floating-point comparisons.

6. **Time Complexity:** O(N) - we visit each element once.
7. **Space Complexity:** O(1) - only a few variables are used.

This problem extends Problem 643's fixed-size sliding window pattern by **counting qualifying windows** instead of finding the maximum, and demonstrates how to **avoid floating-point arithmetic** through mathematical transformation.

**Code:**

```python
class Solution:
    def numOfSubarrays(self, arr: List[int], k: int, threshold: int) -> int:
        target = k * threshold
        res = 0
        curr_sum = sum(arr[:k])
        if curr_sum >= target:
            res += 1
        for i in range(k, len(arr)):
            curr_sum -= arr[i-k]
            curr_sum += arr[i]
            if curr_sum >= target:
                res += 1
        return res
```
