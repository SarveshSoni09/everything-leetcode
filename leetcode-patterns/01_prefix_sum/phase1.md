# Pattern: Prefix Sum - Phase 1

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

## Problem: 1310. XOR Queries of a Subarray

You are given an array `arr` of positive integers. You are also given the array `queries` where `queries[i] = [left_i, right_i]`.

For each query `i` compute the **XOR** of elements from `left_i` to `right_i` (that is, `arr[left_i] XOR arr[left_i + 1] XOR ... XOR arr[right_i]` ).

Return an array `answer` where `answer[i]` is the answer to the `ith` query.

### Example 1

```
Input: arr = [1,3,4,8], queries = [[0,1],[1,2],[0,3],[3,3]]
Output: [2,7,14,8]
Explanation:
The binary representation of the elements in the array are:
1 = 0001
3 = 0011
4 = 0100
8 = 1000
The XOR values for queries are:
[0,1] = 1 xor 3 = 2
[1,2] = 3 xor 4 = 7
[0,3] = 1 xor 3 xor 4 xor 8 = 14
[3,3] = 8
```

### Example 2

```
Input: arr = [4,8,2,10], queries = [[2,3],[1,3],[0,0],[0,3]]
Output: [8,0,4,4]
```

### Constraints

- `1 <= arr.length, queries.length <= 3 \* 10^4`
- `1 <= arr[i] <= 10^9`
- `queries[i].length == 2`
- `0 <= lefti <= righti < arr.length`

### Solution

**Logic:**

This problem is a direct parallel to Problem 303 (Range Sum Query), but it uses the XOR operator instead of addition. The "Prefix Sum" technique can be adapted into a "**Prefix XOR**" array.

The key property that makes this work is that XOR is its own inverse: `A ^ A = 0`.

1. **Build Prefix XOR:** We create a prefix XOR array (the code does this in-place) where `arr[i] = arr[0] ^ arr[1] ^ ... ^ arr[i]`.

2. Query Logic: The XOR of any subarray from `[left, right]` can be found in $O(1)$ time.

- Let `prefix[i] = arr[0] ^ ... ^ arr[i]`.

- We want `query(left, right) = arr[left] ^ ... ^ arr[right]`.

- `prefix[right] = (arr[0] ^ ... ^ arr[left-1]) ^ (arr[left] ^ ... ^ arr[right])`

- `prefix[left-1] = (arr[0] ^ ... ^ arr[left-1])`

- If we XOR these two: `prefix[right] ^ prefix[left-1]`, the common prefix `(arr[0] ^ ... ^ arr[left-1])` cancels itself out, leaving just the desired range.

3. Edge Case: The `code q[0] > 0` correctly handles the left = 0 case, where the answer is just `prefix[right]` (since there is no `prefix[left-1]` to XOR).

**Code:**

```python
class Solution:
    def xorQueries(self, arr: List[int], queries: List[List[int]]) -> List[int]:
        for i in range(len(arr)):
            arr[i] = arr[i] ^ arr[i-1] if i > 0 else arr[i]
        res = []
        for q in queries:
            res.append(arr[q[1]] ^ arr[q[0]-1] if q[0] > 0 else arr[q[1]])

        return res
```

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
        h_map = {0:1}
        for num in nums:
            prefix_sum += num
            if prefix_sum - k in h_map:
                res += h_map[prefix_sum - k]

            if prefix_sum in h_map:
                h_map[prefix_sum] += 1
            else:
                h_map[prefix_sum] = 1

        return res
```

## Problem: 325. Maximum Size Subarray Sum Equals K

Given an integer array `nums` and an integer `k`, return the maximum length of a **contiguous subarray** that sums to `k`. If no such subarray exists, return `0`.

---

### Example 1

```
Input: nums = [1, -1, 5, -2, 3], k = 3
Output: 4
Explanation: The subarray [1, -1, 5, -2] sums to 3 and is the longest.
```

### Example 2

```
Input: nums = [-2, -1, 2, 1], k = 1
Output: 2
Explanation: The subarray [-1, 2] sums to 1 and is the longest.
```

---

### Constraints

- `1 <= nums.length <= 2 * 10^5`
- `-10^4 <= nums[i] <= 10^4`
- `-10^9 <= k <= 10^9`

---

### Solution

**Logic:**

This is the foundational "Prefix Sum + Hash Map" problem for finding **maximum length**.

1.  We want to find `i` and `j` that maximize `j - i + 1` where `prefix_sum[j] - prefix_sum[i-1] = k`.
2.  Rearranging gives: `prefix_sum[i-1] = prefix_sum[j] - k`.
3.  We iterate with `j` (calling it `i` in the code), calculating the `prefix_sum`. The value we _need_ to find in the map is `target = prefix_sum - k`.
4.  To get the **max length**, our map must store `{prefix_sum: first_seen_index}`. By only storing the _first_ (earliest) index, we guarantee that `i - h_map[target]` is the longest possible length.
5.  We initialize `h_map = {0: -1}` to handle subarrays that start from index 0.

**Code:**

```python
class Solution:
    # Function name in LeetCode is maxSubArrayLen
    def maxSubArrayLen(self, nums: List[int], k: int) -> int:
        h_map = {0: -1}
        prefix_sum, res = 0, 0
        for i in range(len(nums)):
            prefix_sum += nums[i]

            target = prefix_sum - k
            if target in h_map:
                if i - h_map[target] > res:
                    res = i - h_map[target]

            # Only store the first occurrence to maximize length
            if prefix_sum not in h_map:
                h_map[prefix_sum] = i

        return res
```
