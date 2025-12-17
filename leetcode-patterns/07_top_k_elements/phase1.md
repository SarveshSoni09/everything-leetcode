# Pattern: Top K Elements - Phase 1

## Problem: 215. Kth Largest Element in an Array

Given an integer array `nums` and an integer `k`, return the `kth` largest element in the array.

Note that it is the `kth` largest element in the sorted order, not the `kth` distinct element.

Can you solve it without sorting?

### Example 1

```
Input: nums = [3,2,1,5,6,4], k = 2
Output: 5
```

### Example 2

```
Input: nums = [3,2,3,1,2,4,5,5,6], k = 4
Output: 4
```

### Constraints

- `1 <= k <= nums.length <= 10^5`
- `-10^4 <= nums[i] <= 10^4`

### Solution

**Logic:**

**Code:**

```python
import heapq
class Solution:
    def findKthLargest(self, nums: List[int], k: int) -> int:
        # Min Heap
        min_heap = []
        for num in nums:
            if len(min_heap) < k:
                heapq.heappush(min_heap, num)
            else:
                heapq.heappushpop(min_heap, num)
        return min_heap[0]
```

Or

```python
class Solution:
    def findKthLargest(self, nums: List[int], k: int) -> int:
        # Quick Select
        k = len(nums) - k

        def quickSelect(l, r):
            pivot, p = nums[r], l
            for i in range(l, r):
                if nums[i] <= pivot:
                    nums[i], nums[p] = nums[p], nums[i]
                    p += 1
            nums[p], nums[r] = nums[r], nums[p]
            print(nums)

            if p > k:   return quickSelect(l, p-1)
            elif p < k: return quickSelect(p+1, r)
            else:       return nums[p]
        return quickSelect(0, len(nums)-1)
```

## Problem: 703. Kth Largest Element in a Stream

You are part of a university admissions office and need to keep track of the `kth` highest test score from applicants in real-time. This helps to determine cut-off marks for interviews and admissions dynamically as new applicants submit their scores.

You are tasked to implement a class which, for a given integer `k`, maintains a stream of test scores and continuously returns the `kth` highest test score after a new score has been submitted. More specifically, we are looking for the `kth` highest score in the sorted list of all scores.

Implement the `KthLargest` class:

- `KthLargest(int k, int[] nums)` Initializes the object with the integer `k` and the stream of test scores nums.
- `int add(int val)` Adds a new test score val to the stream and returns the element representing the `kth` largest element in the pool of test scores so far.

### Example 1

```
Input:
["KthLargest", "add", "add", "add", "add", "add"]
[[3, [4, 5, 8, 2]], [3], [5], [10], [9], [4]]

Output: [null, 4, 5, 5, 8, 8]

Explanation:

KthLargest kthLargest = new KthLargest(3, [4, 5, 8, 2]);
kthLargest.add(3); // return 4
kthLargest.add(5); // return 5
kthLargest.add(10); // return 5
kthLargest.add(9); // return 8
kthLargest.add(4); // return 8
```

### Example 2

```
Input:
["KthLargest", "add", "add", "add", "add"]
[[4, [7, 7, 7, 7, 8, 3]], [2], [10], [9], [9]]

Output: [null, 7, 7, 7, 8]

Explanation:

KthLargest kthLargest = new KthLargest(4, [7, 7, 7, 7, 8, 3]);
kthLargest.add(2); // return 7
kthLargest.add(10); // return 7
kthLargest.add(9); // return 7
kthLargest.add(9); // return 8
```

### Constraints

- `0 <= nums.length <= 10^4`
- `1 <= k <= nums.length + 1`
- `-10^4 <= nums[i] <= 10^4`
- `-10^4 <= val <= 10^4`
- At most `10^4` calls will be made to `add`.

### Solution

**Logic:**

**Code:**

```python
import heapq
class KthLargest:
    def __init__(self, k: int, nums: List[int]):
        self.k = k
        self.min_heap = []
        for num in nums:
            self.add(num)
    def add(self, val: int) -> int:
        if len(self.min_heap) < self.k:
            heapq.heappush(self.min_heap, val)
        else:
            heapq.heappushpop(self.min_heap, val)
        return self.min_heap[0]
```

## 973. K Closest Points to Origin

Given an array of `points` where `points[i] = [xi, yi]` represents a point on the X-Y plane and an integer `k`, return the `k` closest points to the origin `(0, 0)`.

The distance between two points on the X-Y plane is the Euclidean distance (i.e., `√(x1 - x2)2 + (y1 - y2)2)`.

You may return the answer in any order. The answer is guaranteed to be unique (except for the order that it is in).

### Example 1

```
Input: points = [[1,3],[-2,2]], k = 1
Output: [[-2,2]]
Explanation:
The distance between (1, 3) and the origin is sqrt(10).
The distance between (-2, 2) and the origin is sqrt(8).
Since sqrt(8) < sqrt(10), (-2, 2) is closer to the origin.
We only want the closest k = 1 points from the origin, so the answer is just [[-2,2]].
```

### Example 2

```
Input: points = [[3,3],[5,-1],[-2,4]], k = 2
Output: [[3,3],[-2,4]]
Explanation: The answer [[-2,4],[3,3]] would also be accepted.
```

### Constraints

- `1 <= k <= points.length <= 10^4`
- `-10^4 <= xi, yi <= 10^4`

### Solution

**Logic:**

**Code:**

```python
import heapq
class Solution:
    def kClosest(self, points: List[List[int]], k: int) -> List[List[int]]:
        max_heap = []
        res = []
        for x, y in points:
            dist = -((x ** 2) + (y ** 2))
            if len (max_heap) < k:
                heapq.heappush(max_heap, [dist, x, y])
            else:
                heapq.heappushpop(max_heap, [dist, x, y])
        while max_heap:
            dist, x ,y = heapq.heappop(max_heap)
            res.append([x, y])
        return res
```

## Problem: 692. Top K Frequent Words

Given an array of strings words and an integer k, return the k most frequent strings.

Return the answer sorted by the frequency from highest to lowest. Sort the words with the same frequency by their lexicographical order.

Example 1:

Input: words = ["i","love","leetcode","i","love","coding"], k = 2
Output: ["i","love"]
Explanation: "i" and "love" are the two most frequent words.
Note that "i" comes before "love" due to a lower alphabetical order.
Example 2:

Input: words = ["the","day","is","sunny","the","the","the","sunny","is","is"], k = 4
Output: ["the","is","sunny","day"]
Explanation: "the", "is", "sunny" and "day" are the four most frequent words, with the number of occurrence being 4, 3, 2 and 1 respectively.

Constraints:

1 <= words.length <= 500
1 <= words[i].length <= 10
words[i] consists of lowercase English letters.
k is in the range [1, The number of unique words[i]]

Follow-up: Could you solve it in O(n log(k)) time and O(n) extra space?

### Solution

**Logic:**

**Code:**

```python
class Solution:
    def topKFrequent(self, words: List[str], k: int) -> List[str]:
        word_counts = Counter(words)
        max_heap = [(-count, word) for word, count in word_counts.items()]
        heapq.heapify(max_heap)
        res = []
        for _ in range(k):
            res.append(heapq.heappop(max_heap)[1])
        return res
```

## Problem: 347. Top K Frequent Elements

Given an integer array `nums` and an integer `k`, return the `k` most frequent elements. You may return the answer in any order.

### Example 1

```
Input: nums = [1,1,1,2,2,3], k = 2
Output: [1,2]
```

### Example 2

```
Input: nums = [1], k = 1
Output: [1]
```

### Example 3

```
Input: nums = [1,2,1,2,1,2,3,1,3,2], k = 2
Output: [1,2]
```

### Constraints

- `1 <= nums.length <= 10^5`
- `-10^4 <= nums[i] <= 10^4`
- `k` is in the range `[1, the number of unique elements in the array]`.
  It is guaranteed that the answer is unique.

**Follow up:** Your algorithm's time complexity must be better than O(n log n), where n is the array's size.

```python
class Solution:
    import heapq
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        freq = Counter(nums)
        max_heap = [(-count, num) for num, count in freq.items()]
        heapq.heapify(max_heap)
        res = []
        for _ in range(k):
            res.append(heapq.heappop(max_heap)[1])
        return res
```

## Problem: 451. Sort Characters by Frequency

Given a string `s`, sort it in decreasing order based on the frequency of the characters. The frequency of a character is the number of times it appears in the string.

Return the sorted string. If there are multiple answers, return any of them.

### Example 1

```
Input: s = "tree"
Output: "eert"
Explanation: 'e' appears twice while 'r' and 't' both appear once.
So 'e' must appear before both 'r' and 't'. Therefore "eetr" is also a valid answer.
```

### Example 2

```
Input: s = "cccaaa"
Output: "aaaccc"
Explanation: Both 'c' and 'a' appear three times, so both "cccaaa" and "aaaccc" are valid answers.
Note that "cacaca" is incorrect, as the same characters must be together.
```

### Example 3

```
Input: s = "Aabb"
Output: "bbAa"
Explanation: "bbaA" is also a valid answer, but "Aabb" is incorrect.
Note that 'A' and 'a' are treated as two different characters.
```

### Constraints

- `1 <= s.length <= 5 * 10^5`
- `s` consists of uppercase and lowercase English letters and digits.

### Solution

**Logic:**

**Code:**

```python
import heapq
class Solution:
    def frequencySort(self, s: str) -> str:
        char_counts = Counter(s)
        max_heap = [(-count, char) for char, count in char_counts.items()]
        heapq.heapify(max_heap)
        res = []
        for _ in range(len(max_heap)):
            n, c = heapq.heappop(max_heap)
            res += [c] * (-n)

        return ''.join(res)
```
