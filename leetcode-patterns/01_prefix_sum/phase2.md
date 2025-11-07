# Pattern: Prefix Sum - Phase 2

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
