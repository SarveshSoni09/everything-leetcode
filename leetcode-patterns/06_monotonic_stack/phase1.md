# Pattern: Monotonic Stack

## Problem: 496. Next Greater Element I

The next greater element of some element `x` in an array is the first greater element that is to the right of `x` in the same array.

You are given two distinct 0-indexed integer arrays `nums1` and `nums2`, where `nums1` is a subset of `nums2`.

For each `0 <= i < nums1.length`, find the index `j` such that `nums1[i] == nums2[j]` and determine the next greater element of `nums2[j] in nums2`. If there is no next greater element, then the answer for this query is `-1`.

Return an array `ans` of length `nums1`.length such that `ans[i]` is the next greater element as described above.

### Example 1

```
Input: nums1 = [4,1,2], nums2 = [1,3,4,2]
Output: [-1,3,-1]
Explanation: The next greater element for each value of nums1 is as follows:
- 4 is underlined in nums2 = [1,3,4,2]. There is no next greater element, so the answer is -1.
- 1 is underlined in nums2 = [1,3,4,2]. The next greater element is 3.
- 2 is underlined in nums2 = [1,3,4,2]. There is no next greater element, so the answer is -1.
```

### Example 2

```
Input: nums1 = [2,4], nums2 = [1,2,3,4]
Output: [3,-1]
Explanation: The next greater element for each value of nums1 is as follows:
- 2 is underlined in nums2 = [1,2,3,4]. The next greater element is 3.
- 4 is underlined in nums2 = [1,2,3,4]. There is no next greater element, so the answer is -1.
```

### Constraints

- `1 <= nums1.length <= nums2.length <= 1000`
- `0 <= nums1[i], nums2[i] <= 10^4`
- All integers in `nums1` and `nums2` are unique.
- All the integers of `nums1` also appear in `nums2`.

**Follow up**: Could you find an O(nums1.length + nums2.length) solution?

### Solution

**Logic:**

**Code:**

```python
class Solution:
    def nextGreaterElement(self, nums1: List[int], nums2: List[int]) -> List[int]:
        nums1_map = {n : i for i , n in enumerate(nums1)}
        res = [-1] * len(nums1)
        stack = []
        for i in range(len(nums2)):
            curr = nums2[i]
            while stack and curr > stack[-1]:
                val = stack.pop()
                idx = nums1_map[val]
                res[idx] = curr
            if curr in nums1_map:
                stack.append(curr)
        return res
```

## Problem: 739. Daily Temperatures

Given an array of integers `temperatures` represents the daily temperatures, return an array `answer` such that `answer[i]` is the number of days you have to wait after the `ith` day to get a warmer temperature. If there is no future day for which this is possible, keep `answer[i] == 0` instead.

### Example 1

```
Input: temperatures = [73,74,75,71,69,72,76,73]
Output: [1,1,4,2,1,1,0,0]
```

### Example 2

```
Input: temperatures = [30,40,50,60]
Output: [1,1,1,0]
```

### Example 3

```
Input: temperatures = [30,60,90]
Output: [1,1,0]
```

### Constraints

- `1 <= temperatures.length <= 10^5`
- `30 <= temperatures[i] <= 100`

### Solution

**Logic:**

**Code:**

```python
class Solution:
    def dailyTemperatures(self, temperatures: List[int]) -> List[int]:
        answer = [0] * len(temperatures)
        stack = []
        for i, temp in enumerate(temperatures):
            while stack and stack[-1][0] < temp:
                t, idx = stack.pop()
                answer[idx] = i - idx
            stack.append([temp, i])
        return answer
```

## Problem: 503. Next Greater Element II

Given a circular integer array `nums` (i.e., the next element of `nums[nums.length - 1]` is `nums[0]`), return the next greater number for every element in `nums`.

The next greater number of a number `x` is the first greater number to its traversing-order next in the array, which means you could search circularly to find its next greater number. If it doesn't exist, return `-1` for this number.

### Example 1

```
Input: nums = [1,2,1]
Output: [2,-1,2]
Explanation: The first 1's next greater number is 2;
The number 2 can't find next greater number.
The second 1's next greater number needs to search circularly, which is also 2.
```

### Example 2

```
Input: nums = [1,2,3,4,3]
Output: [2,3,4,-1,4]
```

### Constraints

- `1 <= nums.length <= 10^4`
- `-10^9 <= nums[i] <= 10^9`

### Solution

**Logic:**

**Code:**

```python
class Solution:
    def nextGreaterElements(self, nums: List[int]) -> List[int]:
        stack = []
        length = len(nums)
        res = [-1] * len(nums)
        nums *= 2
        for idx, num in enumerate(nums):
            while stack and num > stack[-1][0]:
                n, i = stack.pop()
                res[i] = num
            if idx < length:
                stack.append([num, idx])
        return res
```

Or

```python
class Solution:
    def nextGreaterElements(self, nums: List[int]) -> List[int]:
        n = len(nums)
        result = [-1] * n
        stack = []
        for i in range(2 * n):
            while stack and nums[stack[-1]] < nums[i % n]:
                result[stack.pop()] = nums[i % n]
            if i < n:
                stack.append(i)
        return result
```
