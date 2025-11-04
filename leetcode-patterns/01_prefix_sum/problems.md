# Pattern: Prefix Sum

## Problem: 1480. Running Sum of 1D Array

Given an array `nums`. We define a running sum of an array as `runningSum[i] = sum(nums[0]…nums[i])`.  
Return the running sum of `nums`.

---

### Example 1

**Input:** `nums = [1,2,3,4]`  
**Output:** `[1,3,6,10]`  
**Explanation:** Running sum is obtained as follows: `[1, 1+2, 1+2+3, 1+2+3+4]`.

### Example 2

**Input:** `nums = [1,1,1,1,1]`  
**Output:** `[1,2,3,4,5]`  
**Explanation:** Running sum is obtained as follows: `[1, 1+1, 1+1+1, 1+1+1+1, 1+1+1+1+1]`.

### Example 3

**Input:** `nums = [3,1,2,10,1]`
**Output:** `[3,4,6,16,17]`

---

### Constraints

- `1 <= nums.length <= 1000`
- `-10^6 <= nums[i] <= 10^6`

---

### Solution

**Logic:**

This problem is the most direct implementation of the Prefix Sum pattern.  
The "running sum" is precisely the prefix sum array itself.  
The line `nums[i] += nums[i-1]` is the algorithm for building this array.  
Each `nums[i]` is updated to store the sum of `nums[0]...nums[i]`.  
This fundamental concept is the building block for all the following problems.

**Code:**

```python
class Solution:
    def runningSum(self, nums: List[int]) -> List[int]:
        for i in range(1, len(nums)):
            nums[i] += nums[i-1]
        return nums
```

---

## Problem: 303. Range Sum Query - Immutable

Given an integer array `nums`, handle multiple queries of the following type:

1. Calculate the sum of the elements of `nums` between indices `left` and `right` inclusive where `left <= right`.

Implement the `NumArray` class:

- `NumArray(int[] nums)` Initializes the object with the integer array nums.
- `int sumRange(int left, int right)` Returns the sum of the elements of nums between indices `left` and `right` inclusive (i.e. `nums[left] + nums[left + 1] + ... + nums[right]`).

---

### Example 1

**Input:**

```
["NumArray", "sumRange", "sumRange", "sumRange"]
[[[-2, 0, 3, -5, 2, -1]], [0, 2], [2, 5], [0, 5]]
```

**Output:**

```
[null, 1, -1, -3]
```

**Explanation:**

```
NumArray numArray = new NumArray([-2, 0, 3, -5, 2, -1]);
numArray.sumRange(0, 2); // return (-2) + 0 + 3 = 1
numArray.sumRange(2, 5); // return 3 + (-5) + 2 + (-1) = -1
numArray.sumRange(0, 5); // return (-2) + 0 + 3 + (-5) + 2 + (-1) = -3
```

---

### Constraints

- `1 <= nums.length <= 10^4`
- `-10^5 <= nums[i] <= 10^5`
- `0 <= left <= right < nums.length`
- At most `10^4` calls will be made to `sumRange`.

---

### Solution

**Logic:**

This problem shows _why_ the prefix sum array is so useful. Instead of calculating the sum for each `sumRange` query (which would be $O(N)$ per query), we do one $O(N)$ preprocessing step in `__init__` to build the prefix sum array (just like in Problem 1480).

With this array, `P`, the sum of any subarray from `left` to `right` can be found in $O(1)$ time.

- `P[right]` = `nums[0] + ... + nums[left-1] + nums[left] + ... + nums[right]`

- `P[left-1]` = `nums[0] + ... + nums[left-1]`

By subtracting the two, `P[right] - P[left-1]`, we get `nums[left] + ... + nums[right]`, which is exactly what we want. The `if left == 0` check handles the edge case where we want the sum from the beginning of the array, which is just `P[right]`.

**Code:**

```python
class NumArray:

    def __init__(self, nums: List[int]):
        self.nums = nums
        for i in range(1, len(nums)):
            self.nums[i] += self.nums[i-1]
        print(nums)

    def sumRange(self, left: int, right: int) -> int:
        if left == 0:
            return self.nums[right]
        else:
            return self.nums[right] - self.nums[left-1]
```

---

## Problem: 560. Subarray Sum Equals K

Given an array of integers `nums` and an integer `k`, return the total number of subarrays whose sum equals to `k`.

A subarray is a contiguous _non-empty_ sequence of elements within an array.

---

### Example 1

```
Input: nums = [1,1,1], k = 2
Output: 2
```

### Example 2

```
Input: nums = [1,2,3], k = 3
Output: 2
```

---

### Constraints

- `1 <= nums.length <= 2 * 10^4`
- `-1000 <= nums[i] <= 1000`
- `-10^7 <= k <= 10^7`

---

### Solution

**Logic:**

This is a major evolution of the pattern. We're not just given the range; we have to _find_ ranges that sum to `k`.

We know from Problem 303 that `sum(i, j) = P[j] - P[i-1]`.
The problem is asking us to find all pairs `(i, j)` such that `P[j] - P[i-1] = k`.

We can rearrange this equation to: `P[i-1] = P[j] - k`.

This gives us a new algorithm:

1. Iterate through the array, calculating the prefix sum `P[j]` at each index `j`.

2. At each `j`, we know `P[j]` and `k`. We ask, "How many times have we previously seen a prefix sum `P[i-1]` that equals `P[j] - k`?"

3. A hash map is perfect for this. We store the _frequency_ of each prefix sum we encounter: `{prefix_sum: count}`.

4. The `h_map = {0: 1}` initialization is crucial. It handles cases where the subarray starts from index 0. In this case, `P[j] = k`. Our formula becomes `P[i-1] = P[j] - k = k - k = 0`. By having `h_map[0] = 1`, we correctly count these subarrays.

**Code:**

```python
class Solution:
    def subarraySum(self, nums: List[int], k: int) -> int:
        prefix_sum, res = 0, 0
        h_map = {}
        for num in nums:
            prefix_sum += num
            if prefix_sum - k == 0:
                res += 1
            if prefix_sum - k in h_map:
                res += h_map[prefix_sum - k]

            if prefix_sum in h_map:
                h_map[prefix_sum] += 1
            else:
                h_map[prefix_sum] = 1

        return res
```

---

## Problem: 523. Continuous Subarray Sum

Given an integer array `nums` and an integer `k`, return true if `nums` has a **good subarray** or `false` otherwise.

A **good subarray** is a subarray where:

- its length is at least two, and
- the sum of the elements of the subarray is a multiple of k.

**Note** that:

- A **subarray** is a contiguous part of the array.
- An integer `x` is a multiple of `k` if there exists an integer `n` such that `x = n * k`. `0` is always a multiple of `k`.

---

### Example 1:

```
Input: nums = [23,2,4,6,7], k = 6
Output: true
Explanation: [2, 4] is a continuous subarray of size 2 whose elements sum up to 6.
```

### Example 2:

```
Input: nums = [23,2,6,4,7], k = 6
Output: true
Explanation: [23, 2, 6, 4, 7] is an continuous subarray of size 5 whose elements sum up to 42.
42 is a multiple of 6 because 42 = 7 * 6 and 7 is an integer.
```

### Example 3:

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

---

## Problem: 974. Subarray Sums Divisible by K

Given an integer array `nums` and an integer `k`, return the number of non-empty **subarrays** that have a sum divisible by `k`.

A **subarray** is a **contiguous** part of an array.

---

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

The "Better Solution" is cleaner because the `if remainder == 0: res += 1` check from the first solution is _automatically_ handled by the `h_map = {0: 1}` initialization. When `remainder` is `0`, it finds `h_map[0]`, adds its value (which is `1` the first time), and increments `h_map[0]`. It's a single, elegant loop.

**Code:**

```python
class Solution:
    def subarraysDivByK(self, nums: List[int], k: int) -> int:
        h_map = {}
        pre_sum, res = 0, 0
        for num in nums:
            pre_sum += num
            if pre_sum % k == 0:
                res += 1
            if pre_sum % k in h_map:
                res += h_map[pre_sum % k]
                h_map[pre_sum % k] += 1
            else:
                h_map[pre_sum % k] = 1

        return res
```

**Better Code:**

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
