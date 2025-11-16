# Pattern: Two Pointers - Phase 3

## Problem: 15. 3Sum

Given an integer array `nums`, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, and `j != k`, and `nums[i] + nums[j] + nums[k] == 0`.

Notice that the solution set must not contain duplicate triplets.

### Example 1

```
Input: nums = [-1,0,1,2,-1,-4]
Output: [[-1,-1,2],[-1,0,1]]
Explanation:
nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0.
nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0.
nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0.
The distinct triplets are [-1,0,1] and [-1,-1,2].
Notice that the order of the output and the order of the triplets does not matter.
```

### Example 2

```
Input: nums = [0,1,1]
Output: []
Explanation: The only possible triplet does not sum up to 0.
```

### Example 3

```
Input: nums = [0,0,0]
Output: [[0,0,0]]
Explanation: The only possible triplet sums up to 0.
```

### Constraints

- `3 <= nums.length <= 3000`
- `-10^5 <= nums[i] <= 10^5`

### Solution

**Logic:**

This problem extends **Problem 167 (Two Sum II)** to find **three numbers** that sum to zero. It introduces the concept of **nested loops with two pointers** and **duplicate handling**, which are essential for more complex two-pointer problems.

1. **Building on Problem 167:** Problem 167 found two numbers that sum to a target in a sorted array. This problem finds three numbers that sum to zero by:

   - Fixing one number (outer loop)
   - Using two pointers to find the other two numbers that sum to `-fixed_number`

2. **The Strategy:**

   - **Sort the array first.** This is crucial because:
     - It allows us to use two pointers efficiently
     - It helps us skip duplicates easily
   - **Outer loop:** Iterate through each number `num` at index `i`.
   - **Skip duplicates in outer loop:** If `i > 0` and `nums[i] == nums[i-1]`, skip to avoid duplicate triplets.
   - **Two-pointer search:** For each fixed `num`, use two pointers `l = i+1` and `r = len(nums)-1` to find pairs that sum to `-num`.

3. **Two-Pointer Logic (Inner Loop):**

   - Calculate `threesum = num + nums[l] + nums[r]`.
   - If `threesum > 0`: Decrement `r` to reduce the sum.
   - If `threesum < 0`: Increment `l` to increase the sum.
   - If `threesum == 0`: Add the triplet to results, then:
     - Increment `l` to find the next potential triplet.
     - **Skip duplicates:** While `l < r` and `nums[l] == nums[l-1]`, increment `l`. This ensures we don't add duplicate triplets.

4. **Why Sorting is Essential:**

   - Without sorting, we can't use the two-pointer technique efficiently.
   - Sorting allows us to skip duplicates by comparing adjacent elements.
   - Sorting enables the greedy pointer movement (move left if sum too small, move right if sum too large).

5. **Duplicate Handling:** This is a critical aspect:

   - **Outer loop duplicates:** Skipping `nums[i]` when `nums[i] == nums[i-1]` prevents triplets like `[-1, x, y]` from being found multiple times.
   - **Inner loop duplicates:** After finding a valid triplet, skipping duplicate `nums[l]` values prevents adding `[-1, 0, 1]` multiple times when there are multiple `0`s or `1`s.

6. **Time Complexity:** O(N²) - outer loop is O(N), inner two-pointer search is O(N).
7. **Space Complexity:** O(1) excluding the output array (or O(N) if we count the output).

This problem demonstrates the **nested two-pointer pattern**, where we combine an outer loop with an inner two-pointer search. This pattern is fundamental to problems like 3Sum Closest and 4Sum.

**Code:**

```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        res = []
        nums.sort()

        for i, num in enumerate(nums):
            if i > 0 and num == nums[i-1]:
                continue

            l, r = i+1, len(nums)-1
            while l<r:
                threesum = num + nums[l] + nums[r]
                if threesum > 0:
                    r -= 1
                elif threesum < 0:
                    l += 1
                else:
                    res.append([num, nums[l], nums[r]])
                    l += 1
                    while l<r and nums[l] == nums[l-1]:
                        l+=1
        return res
```

## Problem: 16. 3Sum Closest

Given an integer array `nums` of length `n` and an integer `target`, find three integers in nums such that the sum is closest to target.

Return the sum of the three integers.

You may assume that each input would have exactly one solution.

### Example 1

```
Input: nums = [-1,2,1,-4], target = 1
Output: 2
Explanation: The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).
```

### Example 2

```
Input: nums = [0,0,0], target = 1
Output: 0
Explanation: The sum that is closest to the target is 0. (0 + 0 + 0 = 0).
```

### Constraints

- `3 <= nums.length <= 500`
- `-1000 <= nums[i] <= 1000`
- `-10^4 <= target <= 10^4`

### Solution

**Logic:**

This problem is a variation of **Problem 15 (3Sum)** where instead of finding triplets that sum to exactly zero, we need to find the triplet with the sum **closest** to a target. It introduces the concept of **optimization with two pointers** - tracking the best solution seen so far.

1. **Building on Problem 15:** The structure is identical:

   - Sort the array
   - Outer loop to fix one number
   - Two pointers to find the other two numbers
   - However, we're optimizing for **closeness** rather than exact match

2. **The Strategy:**

   - Initialize `res = float('inf')` to track the **difference** between the current best sum and target (not the sum itself).
   - For each fixed number `num` at index `i`, use two pointers `l = i+1` and `r = len(nums)-1`.
   - Calculate `curr_sum = num + nums[l] + nums[r]`.
   - Calculate `diff = target - curr_sum` (how far we are from target).
   - If `abs(diff) < abs(res)`, update `res = diff` (we found a closer sum).
   - Move pointers based on `diff`:
     - If `diff < 0`: The sum is too large, decrement `r`.
     - If `diff > 0`: The sum is too small, increment `l`.
     - If `diff == 0`: We found the exact target, but we continue to check (though we could break early).

3. **Key Insight:** We track the **difference** (`target - sum`) rather than the sum itself because:

   - We want to minimize `abs(difference)`
   - The sign of `diff` tells us which pointer to move (same logic as Problem 167)
   - At the end, `target - res` gives us the closest sum

4. **Why This Works:** The two-pointer movement is identical to Problem 15 and Problem 167:

   - If current sum is less than target, we need a larger sum → move left pointer right
   - If current sum is greater than target, we need a smaller sum → move right pointer left
   - We explore all possibilities while maintaining the sorted order property

5. **Time Complexity:** O(N²) - same as 3Sum, outer loop O(N) and inner two-pointer search O(N).
6. **Space Complexity:** O(1) - only a few variables are used.

This problem demonstrates **optimization with two pointers** - we're not just finding a match, but finding the **best** match according to some metric (closeness to target). This pattern is useful in many optimization problems.

**Code:**

```python
class Solution:
    def threeSumClosest(self, nums: List[int], target: int) -> int:
        res = float('inf')
        nums.sort()
        for i, num in enumerate(nums):
            l, r = i + 1, len(nums) - 1
            while l < r:
                curr_sum = num + nums[l] + nums[r]
                diff = target - curr_sum
                if abs(diff) < abs(res):
                    res = diff
                if diff < 0:
                    r -= 1
                else:
                    l += 1
        return target - res
```

## Problem: 18. 4Sum

Given an array `nums` of `n` integers, return an array of all the unique quadruplets `[nums[a], nums[b], nums[c], nums[d]]` such that:

- `0 <= a, b, c, d < n`
- `a`, `b`, `c`, and `d` are distinct.
- `nums[a] + nums[b] + nums[c] + nums[d] == target`
- You may return the answer in any order.

### Example 1

```
Input: nums = [1,0,-1,0,-2,2], target = 0
Output: [[-2,-1,1,2],[-2,0,0,2],[-1,0,0,1]]
```

### Example 2

```
Input: nums = [2,2,2,2,2], target = 8
Output: [[2,2,2,2]]
```

### Constraints

- `1 <= nums.length <= 200`
- `-10^9 <= nums[i] <= 10^9`
- `-10^9 <= target <= 10^9`

### Solution

**Logic:**

This problem extends **Problem 15 (3Sum)** to find **four numbers** that sum to a target. It demonstrates **double nesting** with two pointers - two outer loops plus an inner two-pointer search. This shows how the pattern scales to higher dimensions.

1. **Building on Problem 15:** Problem 15 used one outer loop + two pointers. This problem uses **two outer loops + two pointers**:

   - First outer loop: fix `nums[i]`
   - Second outer loop: fix `nums[j]` where `j > i`
   - Two pointers: find `nums[l]` and `nums[r]` such that `nums[i] + nums[j] + nums[l] + nums[r] == target`

2. **The Strategy:**

   - **Sort the array** (essential for two-pointer technique and duplicate skipping).
   - **First outer loop:** Iterate `i` from `0` to `n-4` (we need at least 3 more elements).
     - Skip duplicates: if `i > 0` and `nums[i] == nums[i-1]`, continue.
   - **Second outer loop:** Iterate `j` from `i+1` to `n-3` (we need at least 2 more elements).
     - Skip duplicates: if `j > i+1` and `nums[j] == nums[j-1]`, continue.
   - **Two-pointer search:** For fixed `nums[i]` and `nums[j]`, use `l = j+1` and `r = n-1`:
     - Calculate `curr_sum = nums[i] + nums[j] + nums[l] + nums[r]`.
     - If `curr_sum == target`: Add the quadruplet, then skip duplicates for both `l` and `r`.
     - If `curr_sum > target`: Decrement `r`.
     - If `curr_sum < target`: Increment `l`.

3. **Duplicate Handling:** Critical at multiple levels:

   - **First loop duplicates:** Skip when `nums[i] == nums[i-1]` to avoid `[a, x, y, z]` being found multiple times.
   - **Second loop duplicates:** Skip when `nums[j] == nums[j-1]` (but only if `j > i+1`, not immediately after `i`) to avoid `[a, b, x, y]` duplicates.
   - **After finding a match:** Skip duplicate `nums[l]` and `nums[r]` values to avoid adding the same quadruplet multiple times.

4. **Key Insight:** The pattern generalizes:

   - **K-Sum problems** can be solved with `(K-2)` nested loops + two pointers
   - Each additional number requires one more nested loop
   - The two-pointer technique always handles the last two numbers efficiently

5. **Time Complexity:** O(N³) - two nested loops O(N²) and inner two-pointer search O(N).
6. **Space Complexity:** O(1) excluding the output array.

This problem demonstrates how the two-pointer pattern **scales** to higher dimensions. The nested loop structure shows the general approach for K-Sum problems, making it a valuable template for similar problems.

**Code:**

```python
class Solution:
    def fourSum(self, nums: List[int], target: int) -> List[List[int]]:
        nums.sort()
        n = len(nums)
        res = []
        for i in range(n - 3):
            if i > 0 and nums[i] == nums[i-1]:
                continue
            for j in range(i+1, n - 2):
                if j > i+1 and nums[j] == nums[j-1]:
                    continue
                l, r = j + 1, n - 1
                while l < r:
                    curr_sum = nums[i] + nums[j] + nums[l] + nums[r]
                    if curr_sum - target == 0 and [nums[i], nums[j], nums[l], nums[r]] not in res:
                        res.append([nums[i], nums[j], nums[l], nums[r]] )
                        while l < r and nums[l] == nums[l+1]:
                            l += 1
                        while l < r and nums[r] == nums[r-1]:
                            r -= 1
                        l += 1
                        r -= 1
                    elif curr_sum > target:
                        r -= 1
                    else:
                        l += 1
        return res
```

## Problem: 881. Boats to Save People

You are given an array `people` where `people[i]` is the weight of the `ith` person, and an infinite number of boats where each boat can carry a maximum weight of `limit`. Each boat carries at most two people at the same time, provided the sum of the weight of those people is at most `limit`.

Return the minimum number of boats to carry every given person.

### Example 1

```
Input: people = [1,2], limit = 3
Output: 1
Explanation: 1 boat (1, 2)
```

### Example 2

```
Input: people = [3,2,2,1], limit = 3
Output: 3
Explanation: 3 boats (1, 2), (2) and (3)
```

### Example 3

```
Input: people = [3,5,3,4], limit = 5
Output: 4
Explanation: 4 boats (3), (3), (4), (5)
```

### Constraints

- `1 <= people.length <= 5 * 10^4`
- `1 <= people[i] <= limit <= 3 * 10^4`

### Solution

**Logic:**

This problem combines the two-pointer technique with **greedy algorithm** principles. Unlike previous problems that found specific sums, this problem **minimizes the number of boats** by making optimal pairing decisions.

1. **The Greedy Insight:** To minimize boats, we want to pair the **heaviest** person with the **lightest** person whenever possible. If they can fit together, we save a boat. If not, the heaviest person must go alone.

2. **Two-Pointer Strategy:**

   - **Sort the array** - this allows us to always consider the heaviest and lightest remaining people.
   - Start with `l = 0` (lightest) and `r = len(people) - 1` (heaviest).
   - While `l <= r`:
     - If `people[l] + people[r] <= limit`: They can share a boat. Increment `l` and decrement `r`. Increment boat count.
     - If `people[l] + people[r] > limit`: The heaviest person (`people[r]`) cannot pair with anyone (since everyone else is lighter). The heaviest person goes alone. Decrement `r` only. Increment boat count.

3. **Why This Greedy Approach is Optimal:**

   - If the heaviest person can pair with the lightest, this is always optimal (we use one boat for two people).
   - If the heaviest cannot pair with the lightest, it cannot pair with anyone (all others are lighter), so it must go alone.
   - By always taking the heaviest person and trying to pair them, we ensure optimal boat usage.

4. **Key Insight:** This problem demonstrates **greedy pairing** with two pointers:

   - We're not just finding pairs that sum to a target
   - We're **optimizing** (minimizing boats) by making locally optimal choices
   - The two-pointer technique efficiently finds the best pairing at each step

5. **Edge Case:** When `l == r`, there's one person left who must go alone.

6. **Time Complexity:** O(N log N) - sorting dominates, then O(N) for the two-pointer pass.
7. **Space Complexity:** O(1) - only pointers and a counter are used (assuming in-place sort).

This problem shows how two pointers can be used for **optimization problems** where we want to minimize or maximize something (number of boats) rather than just find matches. The greedy pairing strategy is a powerful technique.

**Code:**

```python
class Solution:
    def numRescueBoats(self, people: List[int], limit: int) -> int:
        people.sort()
        l, r = 0, len(people) - 1
        res = 0
        while l < r:
            if people[l] + people[r] > limit:
                res += 1
                r -= 1
            else:
                res += 1
                l += 1
                r -= 1
        if l == r:
            return res + 1
        return res
```

## Problem: 42. Trapping Rain Water

Given `n` non-negative integers representing an elevation map where the width of each bar is `1`, compute how much water it can trap after raining.

### Example 1

```
Input: height = [0,1,0,2,1,0,1,3,2,1,2,1]
Output: 6
       0
   01110010
 01001000000
Explanation: The above elevation map zeros is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water ones are being trapped.
```

### Example 2

```
Input: height = [4,2,0,3,2,5]
Output: 9
```

### Constraints

- `n == height.length`
- `1 <= n <= 2 * 10^4`
- `0 <= height[i] <= 10^5`

### Solution

**Logic:**

This is one of the most sophisticated two-pointer problems. It combines the two-pointer technique with **dynamic tracking of maximums** and requires understanding how water trapping works. It demonstrates **asymmetric pointer movement** based on local conditions.

1. **Understanding Water Trapping:** Water can be trapped at position `i` only if there are bars higher than `height[i]` on both the left and right. The amount of water at `i` is `min(left_max, right_max) - height[i]`.

2. **The Two-Pointer Insight:** Instead of calculating left_max and right_max for each position separately (which would be O(N²)), we can use two pointers that track the maximums seen so far from each direction.

3. **The Strategy:**

   - Start with `l = 0` and `r = len(height) - 1`.
   - Maintain `l_max` (maximum height seen from the left) and `r_max` (maximum height seen from the right).
   - **Key Insight:** At each step, we process the pointer with the **smaller maximum**:
     - If `l_max <= r_max`: Process the left pointer.
       - The water trapped at `l` is determined by `l_max` (the smaller of the two maxes) because we know there's at least `r_max` (which is >= `l_max`) on the right.
       - Calculate water: `water += l_max - height[l]`.
       - Update `l_max = max(l_max, height[l])`.
       - Increment `l`.
     - If `l_max > r_max`: Process the right pointer (symmetric logic).
       - The water trapped at `r` is determined by `r_max`.
       - Calculate water: `water += r_max - height[r]`.
       - Update `r_max = max(r_max, height[r])`.
       - Decrement `r`.

4. **Why This Works:**

   - By always processing the side with the smaller maximum, we guarantee that the water calculation is correct:
     - If `l_max <= r_max`, we know that `height[l]` is bounded by `l_max` on the left, and there's at least `r_max >= l_max` on the right, so `min(l_max, r_max) = l_max`.
     - The symmetric argument applies when `r_max < l_max`.
   - This allows us to calculate water in a single pass without needing to precompute all left/right maximums.

5. **Key Insight:** This problem demonstrates **asymmetric processing** with two pointers:

   - We don't always move both pointers
   - We choose which pointer to move based on a **comparison of tracked values** (`l_max` vs `r_max`)
   - This is more sophisticated than the symmetric movement in previous problems

6. **Time Complexity:** O(N) - single pass through the array.
7. **Space Complexity:** O(1) - only a few variables are used.

This problem represents the pinnacle of two-pointer technique - it requires:

- Understanding the problem's mathematical structure (water trapping formula)
- Tracking auxiliary information (maximums) while moving pointers
- Making intelligent decisions about which pointer to move based on local conditions
- Combining multiple concepts (two pointers + greedy + optimization)

It's an excellent synthesis of all the two-pointer patterns learned in previous problems.

**Code:**

```python
class Solution:
    def trap(self, height: List[int]) -> int:
        l, r = 0, len(height)-1
        l_max, r_max = height[l], height[r]
        water = 0
        while l < r:
            if l_max <= r_max:
                l += 1
                l_max = max(l_max, height[l])
                water += l_max - height[l]
            else:
                r -= 1
                r_max = max(r_max, height[r])
                water += r_max - height[r]
        return water
```
