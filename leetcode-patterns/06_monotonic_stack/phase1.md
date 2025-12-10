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

## Problem: 901. Online Stock Span

Design an algorithm that collects daily price quotes for some stock and returns the span of that stock's price for the current day.

The span of the stock's price in one day is the maximum number of consecutive days (starting from that day and going backward) for which the stock price was less than or equal to the price of that day.

- For example, if the prices of the stock in the last four days is `[7,2,1,2]` and the price of the stock today is 2, then the span of today is 4 because starting from today, the price of the stock was less than or equal 2 for 4 consecutive days.
- Also, if the prices of the stock in the last four days is `[7,34,1,2]` and the price of the stock today is `8`, then the span of today is `3` because starting from today, the price of the stock was less than or equal `8` for `3` consecutive days.
  Implement the `StockSpanner` class:

- `StockSpanner()` Initializes the object of the class.
- `int next(int price)` Returns the span of the stock's price given that today's price is price.

### Example 1

```
Input
["StockSpanner", "next", "next", "next", "next", "next", "next", "next"]
[[], [100], [80], [60], [70], [60], [75], [85]]
Output
[null, 1, 1, 1, 2, 1, 4, 6]

Explanation
StockSpanner stockSpanner = new StockSpanner();
stockSpanner.next(100); // return 1
stockSpanner.next(80);  // return 1
stockSpanner.next(60);  // return 1
stockSpanner.next(70);  // return 2
stockSpanner.next(60);  // return 1
stockSpanner.next(75);  // return 4, because the last 4 prices (including today's price of 75) were less than or equal to today's price.
stockSpanner.next(85);  // return 6
```

### Constraints

- `1 <= price <= 10^5`
- At most `10^4` calls will be made to next.

### Solution

**Logic:**

**Code:**

```python
class StockSpanner:

    def __init__(self):
        self.stack = []

    def next(self, price: int) -> int:
        span = 1
        while self.stack and self.stack[-1][0] <= price:
            span += self.stack[-1][1]
            self.stack.pop()
        self.stack.append([price, span])
        return span
```

## Problem: 1019. Next Greater Node In Linked List

You are given the `head` of a linked list with `n` nodes.

For each node in the list, find the value of the next greater node. That is, for each node, find the value of the first node that is next to it and has a strictly larger value than it.

Return an integer array `answer` where `answer[i]` is the value of the next greater node of the `ith` node (1-indexed). If the `ith` node does not have a next greater node, set `answer[i] = 0`.

### Example 1

```
Input: head = [2,1,5]
Output: [5,5,0]
```

### Example 2

```
Input: head = [2,7,4,3,5]
Output: [7,0,5,5,0]
```

### Constraints

- The number of nodes in the list is `n`.
- `1 <= n <= 10^4`
- `1 <= Node.val <= 10^9`

### Solution

**Logic:**

**Code:**

```python
class Solution:
    def nextLargerNodes(self, head: Optional[ListNode]) -> List[int]:
        curr = head
        nums = []
        while curr:
            nums.append(curr.val)
            curr = curr.next
        stack = []
        answer = [0] * len(nums)
        for i, n in enumerate(nums):
            while stack and stack[-1][0] < n:
                idx = stack.pop()[1]
                answer[idx] = n
            stack.append([n, i])
        return answer
```

Or

```python
class Solution:
    def nextLargerNodes(self, head: Optional[ListNode]) -> List[int]:
        res = []
        stack = []
        index = 0
        curr = head
        while curr:
            res.append(0)
            while stack and stack[-1][1] < curr.val:
                i, value = stack.pop()
                res[i] = curr.val
            stack.append([index, curr.val])
            index +=1
            curr = curr.next
        return res
```

## 1762. Buildings With an Ocean View

You are given an array `heights` representing the heights of `n` buildings arranged in a line from left to right. The ocean is located to the right of the last building.

A building has an "ocean view" if it can see the ocean without any obstruction. Formally, a building at index $i$ has an ocean view if all buildings to its right (i.e., at indices $j > i$) are strictly shorter than it is.

Your task is to find all buildings that have an ocean view and return their indices (0-indexed) in ascending (left-to-right) order.

### Example 1

```
Input: heights = [4, 2, 3, 1]
Output: [0, 2, 3]
Explanation: Building 0 (height 4) has an ocean view because all buildings to its right (2, 3, 1) are shorter.
Building 2 (height 3) has an ocean view because the only building to its right (1) is shorter.
Building 3 (height 1) is the rightmost building, so it always has an ocean view.
```

### Example 2

```
Input: heights = [1, 3, 2, 4]
Output: [3]
Explanation: Only the last building (height 4) has an ocean view.
Building 1 (height 3) is blocked by building 3 (height 4).
```

### Solution

**Logic:**

**Code:**

```python
class Solution:
    def oceanView(self, heights: List[int]) -> List[int]:
        stack = []
        answer = []
        for i, n in enumerate(heights):
            while stack and stack[-1][0] < n:
                stack.pop()[1]
            stack.append([n, i])
        for h in stack:
            answer.append(h[1])`
        return answer
```

## Problem: 84. Largest Rectangle in Histogram

Given an array of integers `heights` representing the histogram's bar height where the width of each bar is `1`, return the area of the largest rectangle in the histogram.

### Example 1

```
Input: heights = [2,1,5,6,2,3]
Output: 10
Explanation: The above is a histogram where width of each bar is 1.
The largest rectangle is shown in the red area, which has an area = 10 units.
```

### Example 2

```
Input: heights = [2,4]
Output: 4
```

### Constraints

- `1 <= heights.length <= 10^5`
- `0 <= heights[i] <= 10^4`

### Solution

**Logic:**

**Code:**

```python
class Solution:
    def largestRectangleArea(self, heights: List[int]) -> int:
        max_area = 0
        stack = []
        for idx, height in enumerate(heights):
            start = idx
            while stack and stack[-1][1] > height:
                i, h = stack.pop()
                max_area = max(max_area, h * (idx - i))
                start = i
            stack.append([start, height])
        for idx, height in stack:
            max_area = max(max_area, height * (len(heights) - idx))
        return max_area
```
