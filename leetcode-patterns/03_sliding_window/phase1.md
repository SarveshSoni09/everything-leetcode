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

### Constraints

- `1 <= s.length, p.length <= 3 * 10^4`
- `s` and `p` consist of lowercase English letters.

### Solution

**Logic:**

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
