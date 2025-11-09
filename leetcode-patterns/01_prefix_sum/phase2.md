# Pattern: Prefix Sum - Phase 2

## Problem: 930. Binary Subarray with Sum

Given a binary array nums and an integer goal, return the **number** of non-empty subarrays with a sum goal.

A subarray is a contiguous part of the array.

### Example 1:

```
Input: nums = [1,0,1,0,1], goal = 2
Output: 4
Explanation: The 4 subarrays are bolded and underlined below:
[1,0,1,0,1]
[1,0,1,0,1]
[1,0,1,0,1]
[1,0,1,0,1]
```

---

### Constraints:

- `1 <= nums.length <= 3 * 10^4`
- `nums[i] is either 0 or 1.`
- `0 <= goal <= nums.length`

### Solution

**Logic:**

This problem also builds on the logic of **Problem 325**, but with one critical change.

1.  **The Variation:** We need the **total count** of subarrays, not the **maximum length**.
2.  **The Extra Step:** Because we aren't maximizing length, we no longer care about the _first index_. Instead, we need to know _how many times_ each `prefix_sum` has occurred.
3.  **Why?** The same `target = prefix_sum - goal` logic applies. But if our `target` has appeared 3 times before, that means there are 3 distinct subarrays ending at our current index `i` that sum to `goal`.
4.  **Result:** The map must store `{prefix_sum: frequency_count}`.
5.  **Algorithm Change:**
    - When `target` is in `h_map`, add `h_map[target]` to `res`.
    - Update the _current_ `prefix_sum`'s count in the map.
    - We initialize `h_map = {0: 1}` to handle subarrays that start from index 0.

**Code:**

```python
class Solution:
    def numSubarraysWithSum(self, nums: List[int], goal: int) -> int:
        h_map = {0: 1} # {prefix_sum: frequency}
        prefix_sum, res = 0, 0
        for i in range(len(nums)):
            prefix_sum += nums[i]

            target = prefix_sum - goal
            if target in h_map:
                res += h_map[target]

            # Update the frequency of the current prefix_sum
            if prefix_sum in h_map:
                h_map[prefix_sum] += 1
            else:
                h_map[prefix_sum] = 1

        return res
```

## Problem: 1248. Count Number of Nice Subarrays

Given an array of integers `nums` and an integer `k`. A continuous subarray is called **nice** if there are `k` odd numbers on it.

Return _the number of **nice** sub-arrays_.

### Example 1

```
Input: nums = [1,1,2,1,1], k = 3
Output: 2
Explanation: The only sub-arrays with 3 odd numbers are [1,1,2,1] and [1,2,1,1].
```

### Example 2

```
Input: nums = [2,4,6], k = 1
Output: 0
Explanation: There are no odd numbers in the array.
```

### Example 3

```
Input: nums = [2,2,2,1,2,2,1,2,2,2], k = 2
Output: 16
```

### Solution

**Logic:**

This problem is a clever disguise for the "Subarray Sum Equals K" pattern (Problem 560/930).

1. **The Transformation:** The key insight is to transform the array. We don't care about the values of the numbers, only whether they are odd or even. The code does this transformation:

- `odd number -> 1`
- `even number -> 0`

2. **The New Problem:** After this transformation, the original problem "find the number of subarrays with `k` odd numbers" becomes "find the number of subarrays with a `sum` of `k`."

3. Standard Solution: We can now apply the standard 1D prefix sum + hash map algorithm for counting:

- `presum` tracks the running prefix sum, which now represents the count of odd numbers seen so far.
- The `h_map` stores the frequency of each `presum` count.
- For each new `presum` at index `i`, we look for `presum - k` in the map. The value `h_map[presum - k]` tells us how many valid subarrays ending at index `i` have `k` odd numbers.
- `h_map = {0: 1}` is the standard initialization to correctly count subarrays that start from the beginning of the array.

**Code:**

```python
class Solution:
    def numberOfSubarrays(self, nums: List[int], k: int) -> int:
        presum, res = 0, 0
        h_map = {0:1}

        for i in range(len(nums)):
            nums[i] = 0 if nums[i] % 2 == 0 else 1
            presum += nums[i]
            if presum - k in h_map:
                res += h_map[presum - k]
            if presum in h_map:
                h_map[presum] += 1
            else:
                h_map[presum] = 1

        return res
```

## Problem: 525. Contiguous Subarray

Given a binary array `nums`, return the maximum length of a **contiguous subarray** with an equal number of `0` and `1`.

### Example 1:

```
Input: nums = [0,1]
Output: 2
Explanation: [0, 1] is the longest contiguous subarray with an equal number of 0 and 1.
```

### Example 2:

```
Input: nums = [0,1,0]
Output: 2
Explanation: [0, 1] (or [1, 0]) is a longest contiguous subarray with equal number of 0 and 1.
```

---

### Constraints:

- `1 <= nums.length <= 105`
- `nums[i] is either 0 or 1.`

### Solution

**Logic:**

This problem is a clever variation of **Problem 325**.

1.  **The Variation:** We need equal 0s and 1s.
2.  **The Extra Step:** We can transform this into a "sum" problem by **treating all `0`s as `-1`**.
3.  **Why?** A subarray with an equal number of `1`s and `-1`s will have a sum of `0`.
4.  **Result:** The problem is now "Find the maximum length subarray with sum `k = 0`." This is exactly **Problem 325** with `k=0`. The rest of the logic (using a map to store the _first_ index to maximize length) is identical.

**Code:**

```python
class Solution:
    def findMaxLength(self, nums: List[int]) -> int:
        h_map = {0: -1}
        prefix_sum, res = 0, 0
        for i in range(len(nums)):
            # Treat 0 as -1
            prefix_sum += nums[i] if nums[i] == 1 else -1

            if prefix_sum in h_map:
                # If sum is seen, subarray from h_map[prefix_sum]+1 to i has sum 0
                if i - h_map[prefix_sum] > res:
                    res = i - h_map[prefix_sum]
            else:
                # Only store the first time we see a prefix_sum
                h_map[prefix_sum] = i

        return res
```

## Problem: 523. Continuous Subarray Sum

Given an integer array `nums` and an integer `k`, return true if `nums` has a **good subarray** or `false` otherwise.

A **good subarray** is a subarray where:

- its length is at least two, and
- the sum of the elements of the subarray is a multiple of k.

**Note** that:

- A **subarray** is a contiguous part of the array.
- An integer `x` is a multiple of `k` if there exists an integer `n` such that `x = n * k`. `0` is always a multiple of `k`.

---

### Example 1

```
Input: nums = [23,2,4,6,7], k = 6
Output: true
Explanation: [2, 4] is a continuous subarray of size 2 whose elements sum up to 6.
```

### Example 2

```
Input: nums = [23,2,6,4,7], k = 6
Output: true
Explanation: [23, 2, 6, 4, 7] is an continuous subarray of size 5 whose elements sum up to 42.
42 is a multiple of 6 because 42 = 7 * 6 and 7 is an integer.
```

### Example 3

```
Input: nums = [23,2,6,4,7], k = 13
Output: false
```

---

### Constraints:

- `1 <= nums.length <= 10^5`
- `0 <= nums[i] <= 10^9`
- `0 <= sum(nums[i]) <= 2^31 - 1`
- `1 <= k <= 2^31 - 1`

---

### Solution

**Logic:**

This problem adds a twist: we're looking for a sum that is a _multiple of k_, not equal to `k`.

We want `sum(i, j) = n * k` for some integer `n`.
Using prefix sums: `P[j] - P[i-1] = n * k`.

This equation means that `P[j]` and `P[i-1]` must have the **same remainder** when divided by `k`.

- If `P[j] % k = rem`

- And `P[i-1] % k = rem`

- Then `(P[j] - P[i-1]) % k = (rem - rem) % k = 0`. This proves their difference is a multiple of `k`.

So, the algorithm changes:

1. The hash map now stores `{remainder: index}`. We only need to store the _first_ index where we see a remainder.

2. We iterate, calculating `prefix_sum` and its `remainder = prefix_sum % k`.

3. If we find `remainder` in `h_map`, it means we've seen it before at index `h_map[remainder]`. The subarray between `h_map[remainder] + 1` and `i` has a sum that is a multiple of `k`.

4. We check if its length `i - h_map[remainder]` is at least 2. If it is, we return `True`.

5. The `h_map = {0: -1}` initialization is key. If `P[j]` itself is a multiple of `k`, its remainder is 0. We find `0` in the map, and the length is `i - (-1) = i + 1`. This correctly validates subarrays starting from index 0.

**Code:**

```python
class Solution:
    def checkSubarraySum(self, nums: List[int], k: int) -> bool:
        h_map = {0: -1}
        prefix_sum, res = 0, 0
        for i in range(len(nums)):
            prefix_sum += nums[i]
            if prefix_sum % k in h_map:
                if i - h_map[prefix_sum % k] >= 2:
                    return True
            else:
                h_map[prefix_sum % k] = i
        return False

```

## Problem: 974. Subarray Sums Divisible by K

Given an integer array `nums` and an integer `k`, return the number of non-empty **subarrays** that have a sum divisible by `k`.

A **subarray** is a **contiguous** part of an array.

### Example 1

```
Input: nums = [4,5,0,-2,-3,1], k = 5
Output: 7
Explanation: There are 7 subarrays with a sum divisible by k = 5:
[4, 5, 0, -2, -3, 1], [5], [5, 0], [5, 0, -2, -3], [0], [0, -2, -3], [-2, -3]
```

### Example 2

```
Input: nums = [5], k = 9
Output: 0
```

---

### Constraints

```
1 <= nums.length <= 3 * 10^4
-10^4 <= nums[i] <= 10^4
2 <= k <= 10^4
```

---

### Solution

**Logic:**

This problem is the final synthesis. It combines the _counting_ logic from Problem 560 with the _modular arithmetic_ logic from Problem 523.

- We need the **count** of subarrays, so our hash map must store **frequencies** (like 560): `{key: frequency}`.

- We are looking for sums **divisible by k**, so the key to our map must be the **remainder** (like 523): `{remainder: frequency}`.

The logic is:

1. Initialize `h_map = {0: 1}` and `res = 0`. The `{0: 1}` entry is our "super-tool." It represents the empty prefix (sum 0, remainder 0) that occurred once.

2. Iterate, calculating `pre_sum` and its `remainder`.

3. When we get a new `remainder` (from `P[j]`), we ask, "How many times have we seen this _same remainder_ before?"

4. The answer is `h_map[remainder]`. Every previous time we saw it (at an index `i-1`), it forms a valid pair `(i, j)` because `P[j] % k == P[i-1] % k`.

5. We add this count to our result: `res += h_map[remainder]`.

6. Then, we update the map with our current remainder: `h_map[remainder] = h_map.get(remainder, 0) + 1`.

**Code:**

```python
class Solution:
    def subarraysDivByK(self, nums: List[int], k: int) -> int:
        h_map = {0: 1}
        pre_sum, res = 0, 0
        for num in nums:
            pre_sum += num
            if pre_sum % k in h_map:
                res += h_map[pre_sum % k]
                h_map[pre_sum % k] += 1
            else:
                h_map[pre_sum % k] = 1

        return res
```
