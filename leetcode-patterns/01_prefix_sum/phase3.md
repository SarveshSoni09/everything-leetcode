# Pattern: Prefix Sum - Phase 3

## Problem: 304. Range Sum Query 2D - Immutable

Given a 2D matrix `matrix`, handle multiple queries of the following type:

- Calculate the sum of the elements of `matrix` inside the rectangle defined by its **upper left corner** `(row1, col1)` and **lower right corner** `(row2, col2)`.

Implement the `NumMatrix` class for $O(1)$ query time.

### Example 1:

```
Input
["NumMatrix", "sumRegion", "sumRegion", "sumRegion"]
[[[[3, 0, 1, 4, 2], [5, 6, 3, 2, 1], [1, 2, 0, 1, 5], [4, 1, 0, 1, 7], [1, 0, 3, 0, 5]]], [2, 1, 4, 3], [1, 1, 2, 2], [1, 2, 2, 4]]
Output
[null, 8, 11, 12]
```

### Constraints:

- `1 <= m, n <= 200`
- `0 <= row1 <= row2 < m`
- `0 <= col1 <= col2 < n`
- At most `104` calls will be made to `sumRegion`.

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
