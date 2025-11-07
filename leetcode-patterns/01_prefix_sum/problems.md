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

---

## Problem: 525. Contiguous Subarray

Given a binary array `nums`, return the maximum length of a **contiguous subarray** with an equal number of `0` and `1`.

---

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

---

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

---

## Problem: 930. Binary Subarray with Sum

Given a binary array nums and an integer goal, return the **number** of non-empty subarrays with a sum goal.

A subarray is a contiguous part of the array.

---

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

---

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

---

## Problem: 304. Range Sum Query 2D - Immutable

Given a 2D matrix `matrix`, handle multiple queries of the following type:

- Calculate the sum of the elements of `matrix` inside the rectangle defined by its **upper left corner** `(row1, col1)` and **lower right corner** `(row2, col2)`.

Implement the `NumMatrix` class for $O(1)$ query time.

---

### Example 1:

```
Input
["NumMatrix", "sumRegion", "sumRegion", "sumRegion"]
[[[[3, 0, 1, 4, 2], [5, 6, 3, 2, 1], [1, 2, 0, 1, 5], [4, 1, 0, 1, 7], [1, 0, 3, 0, 5]]], [2, 1, 4, 3], [1, 1, 2, 2], [1, 2, 2, 4]]
Output
[null, 8, 11, 12]
```

---

### Constraints:

- `1 <= m, n <= 200`
- `0 <= row1 <= row2 < m`
- `0 <= col1 <= col2 < n`
- At most `104` calls will be made to `sumRegion`.

---

### Solution

**Logic:**

This problem extends the 1D prefix sum concept (like in `NumArray`) to 2D. It is _not_ related to the hash map pattern. The goal is to precompute a 2D prefix sum matrix in $O(M*N)$ to enable $O(1)$ queries.

1.  **The Variation:** Prefix sum in 2D.
2.  **The Extra Step:** We build a 2D matrix `prefix_mat` where `prefix_mat[r][c]` stores the sum of the rectangle from `(0, 0)` to `(r-1, c-1)`.
3.  **Build Logic:** We use the **principle of inclusion-exclusion**. The sum for the rectangle ending at `[r][c]` is:
    `Sum = matrix[r][c] + Sum_Above + Sum_Left - Sum_TopLeft`
    `prefix_mat[r+1][c+1] = matrix[r][c] + prefix_mat[r][c+1] + prefix_mat[r+1][c] - prefix_mat[r][c]`
4.  **Query Logic:** We use the same principle to "cut out" the desired rectangle. We use `+1` on `row2` and `col2`, and _not_ on `row1` and `col1`, because our `prefix_mat` is padded by 1.
    `Sum = Total_Rect - Top_Rect - Left_Rect + TopLeft_Rect`
    `ans = pm[r2+1][c2+1] - pm[r1][c2+1] - pm[r2+1][c1] + pm[r1][c1]`

**Code:**

```python
class NumMatrix:

    def __init__(self, matrix: List[List[int]]):
        m = len(matrix)
        n = len(matrix[0])
        # Create a (m+1) x (n+1) matrix to simplify edge cases
        prefix_mat = [[0] * (n+1) for _ in range(m+1)]

        # Build the 2D prefix sum matrix
        for row in range(m):
            for col in range(n):
                prefix_mat[row+1][col+1] = (
                    matrix[row][col]
                    + prefix_mat[row][col+1]   # Sum of region above
                    + prefix_mat[row+1][col]   # Sum of region to the left
                    - prefix_mat[row][col]     # Subtract top-left, which was added twice
                )
        self.prefix_mat = prefix_mat

    def sumRegion(self, row1: int, col1: int, row2: int, col2: int) -> int:
        pm = self.prefix_mat

        # Use inclusion-exclusion to find the sum
        ans = (
            pm[row2+1][col2+1]  # The "total" rectangle from (0,0)
            - pm[row1][col2+1]    # Subtract region above
            - pm[row2+1][col1]    # Subtract region to the left
            + pm[row1][col1]      # Add back top-left corner
        )
        return ans
```

## Problem: 1314. Matrix Block Sum

Given a `m x n` matrix `mat` and an integer `k`, return a matrix `answer` where each `answer[i][j]` is the sum of all elements `mat[r][c]` for:

- `i - k <= r <= i + k,`
- `j - k <= c <= j + k,` and
- `(r, c)` is a valid position in the matrix.

### Example 1

```
Input: mat = [[1,2,3],[4,5,6],[7,8,9]], k = 1
Output: [[12,21,16],[27,45,33],[24,39,28]]
```

### Example 2

```
Input: mat = [[1,2,3],[4,5,6],[7,8,9]], k = 2
Output: [[45,45,45],[45,45,45],[45,45,45]]
```

### Constraints

- `m == mat.length`
- `n == mat[i].length`
- `1 <= m, n, k <= 100`
- `1 <= mat[i][j] <= 100`

### Solution

**Logic:**

This is a classic 2D prefix sum problem. A naive solution would calculate the sum for each cell `ans[r][c]` by iterating through its `(2k+1) x (2k+1)` block, which would be $O(m \cdot n \cdot k^2)$. We can do much better.

The goal is to answer $O(mn)$ rectangle-sum queries efficiently.

1. **Preprocessing:** First, precompute a 2D prefix sum matrix, `prefix_mat`, in $O(mn)$ time. We use a 1-indexed `(m+1) x (n+1)` matrix to simplify boundary lookups (the 0th row and 0th col are all zeros). `prefix_mat[r+1][c+1]` will store the sum of all elements from `mat[0][0]` to `mat[r][c]`.

2. **Building `prefix_mat`:** The formula is the standard inclusion-exclusion principle:
   `prefix_mat[r+1][c+1] = mat[r][c] + prefix_mat[r][c+1] + prefix_mat[r+1][c] - prefix_mat[r][c]`.

3. **Answering Queries:** Create an `ans` matrix. Iterate through each cell `(r, c)` in the _original_ matrix.

4. **Clamp Coordinates:** For each `(r, c)`, determine the _actual_ bounds of the subgrid. We must "clamp" the coordinates to stay within the matrix, as the block for cells near the edge will be smaller.

   - Top-left corner `(r1, c1) = (max(0, r-K), max(0, c-K))`

   - Bottom-right corner `(r2, c2) = (min(m-1, r+K), min(n-1, c+K))`

5. **Query in** $O(1)$**:** Use the standard 2D prefix sum query (inclusion-exclusion) to find the sum of this `(r1, c1)` to `(r2, c2)` block in $O(1)$ time.

   - The formula is: `sum = prefix_mat[r2+1][c2+1] - prefix_mat[r1][c2+1] - prefix_mat[r2+1][c1] + prefix_mat[r1][c1]`.

   - This formula works perfectly even at the edges because of our 1-indexed `prefix_mat` (e.g., if `r1=0`, `prefix_mat[r1][...]` will correctly be `0`).

6. **Assign Sum:** Set `ans[r][c] = sum` and return `ans`.

**Code:**

```python
class Solution:
    def matrixBlockSum(self, mat: List[List[int]], K: int) -> List[List[int]]:

        m = len(mat)
        n = len(mat[0])
        prefix_mat = [[0] * (n+1) for _ in range(m+1)]

        for r in range(m):
            for c in range(n):
                prefix_mat[r+1][c+1] = (
                    mat[r][c] +
                    prefix_mat[r][c+1] +
                    prefix_mat[r+1][c] -
                    prefix_mat[r][c]
                )

        ans = [[0] * n for _ in range(m)]

        for r in range(m):
            for c in range(n):
                r1 = max(0, r-K)
                c1 = max(0, c-K)
                r2 = min(m-1, r+K)
                c2 = min(n-1, c+K)

                ans[r][c] = (
                    prefix_mat[r2+1][c2+1]
                    - prefix_mat[r1][c2+1]
                    - prefix_mat[r2+1][c1]
                    + prefix_mat[r1][c1]
                )

        return ans
```

## Problem: 1074. Number of Submatrices That Sum to Target

Given a `matrix` and a `target`, return the number of non-empty submatrices that sum to target.

A submatrix `x1, y1, x2, y2` is the set of all cells `matrix[x][y]` with `x1 <= x <= x2` and `y1 <= y <= y2`.

Two submatrices `(x1, y1, x2, y2)` and `(x1', y1', x2', y2')` are different if they have some coordinate that is different: for example, if `x1 != x1'`.

### Example 1

```
Input: matrix = [[0,1,0],[1,1,1],[0,1,0]], target = 0
Output: 4
Explanation: The four 1x1 submatrices that only contain 0.
```

### Example 2

```
Input: matrix = [[1,-1],[-1,1]], target = 0
Output: 5
Explanation: The two 1x2 submatrices, plus the two 2x1 submatrices, plus the 2x2 submatrix.
```

### Example 3:

```
Input: matrix = [[904]], target = 0
Output: 0
```

### Constraints

- `1 <= matrix.length <= 100`
- `1 <= matrix[0].length <= 100`
- `-1000 <= matrix[i][j] <= 1000`
- `-10^8 <= target <= 10^8`

### Solution

**Logic:**

This problem cleverly reduces a 2D problem into multiple 1D problems, with a total time complexity of $O(m^2 \cdot n)$.

1. **Preprocessing:** First, precompute a 2D prefix sum matrix, `prefix_mat`, in $O(mn)$ time. The code uses a 0-indexed matrix, where `prefix_mat[r][c]` stores the sum of the rectangle from `(0,0)` to `(r,c)`. The `if r > 0` checks handle the boundary conditions.

2. **Iterate Row Pairs:** Iterate through all possible top and bottom row pairs, `r1` and `r2`, where `r2 >= r1`. This costs $O(m^2)$ and defines the vertical bounds of all potential submatrices.

3. **Reduce to 1D Problem:** For _each_ row pair (`r1`, `r2`), we now solve a 1D "Subarray Sum Equals K" problem. The 'array' we are scanning is the "squished" row, where each element represents the sum of a column from `r1` to `r2`.

4. **Solve 1D Problem in** $O(n)$**:**

   - Initialize a hash map `h_map = {0: 1}`. This map will store the frequencies of prefix sums _for this 1D 'squished' array_. It is reset for every new row pair.

   - Iterate through each column `c` from `0` to `n-1`.

   - Calculate `sub_sum`: This `sub_sum` is the sum of the rectangle from `(r1, 0)` to `(r2, c)`. We find this in $O(1)$ using our `prefix_mat`:
     `sub_sum = prefix_mat[r2][c] - (prefix_mat[r1-1][c] if r1 > 0 else 0)`

   - This `sub_sum` acts as the prefix sum for our 'squished' 1D array (from column `0` to `c`).

   - **Apply 1D Logic:** We now apply the standard "Subarray Sum Equals K" hash map logic:

     - We need a `diff = sub_sum - target`.

     - If `diff` is in `h_map`, it means there are `h_map[diff]` subarrays _ending_ at column `c` (and starting at some column `c_left <= c`) that sum to `target`.

     - Add this frequency to our total `res`: `res += h_map.get(diff, 0)`.

     - Update the `h_map` with the `sub_sum` we just saw: `h_map[sub_sum] = h_map.get(sub_sum, 0) + 1`.

**Code:**

```python
class Solution:
    def numSubmatrixSumTarget(self, matrix: List[List[int]], target: int) -> int:
        m = len(matrix)
        n = len(matrix[0])
        prefix_mat = [[0] * (n) for _ in range(m)]
        res = 0

        for r in range(m):
            for c in range(n):
                prefix_mat[r][c] = (
                    matrix[r][c]
                    + (prefix_mat[r-1][c] if r > 0 else 0)
                    + (prefix_mat[r][c-1] if c > 0 else 0)
                    - (prefix_mat[r-1][c-1] if min(r, c) > 0 else 0)
                )

        for r1 in range(m):
            for r2 in range(r1, m):
                h_map = {0:1}
                for c in range(n):
                    sub_sum = prefix_mat[r2][c] - (prefix_mat[r1-1][c] if r1 > 0 else 0)
                    if sub_sum - target in h_map:
                        res += h_map[sub_sum - target]
                    if sub_sum in h_map:
                        h_map[sub_sum] += 1
                    else:
                        h_map[sub_sum] = 1

        return res
```
