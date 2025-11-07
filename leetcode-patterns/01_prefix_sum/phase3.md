# Pattern: Prefix Sum - Phase 3

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
