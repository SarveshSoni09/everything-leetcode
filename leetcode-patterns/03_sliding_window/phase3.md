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

