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

**Min Heap Approach:**
The key insight is to maintain a min heap of size `k`. The heap stores the `k` largest elements seen so far, with the smallest of these `k` elements at the root.

1. **Maintain Size-K Heap:** For each number in the array:

   - If the heap has fewer than `k` elements, add the number
   - Otherwise, use `heappushpop` to add the number and remove the smallest element
   - This ensures we always keep only the `k` largest elements

2. **Result:** After processing all elements, the root of the min heap is the `kth` largest element (the smallest among the `k` largest).

**Why Min Heap for Kth Largest:** When finding the `kth` largest, we want to discard smaller elements. A min heap of size `k` naturally keeps the `k` largest elements, with the smallest of them (the `kth` largest) at the root.

**Time Complexity:** O(N log k) where N is the array length. We process N elements, and each heap operation (push/pop) takes O(log k) time since the heap size never exceeds `k`.

**Space Complexity:** O(k) for the heap.

**Quick Select Approach:**
Uses the partition technique from quicksort to find the `kth` smallest element in one pass, then converts it to `kth` largest by using index `len(nums) - k`.

**Time Complexity:** O(N) average case, O(N²) worst case. We partition the array, and on average we eliminate half the elements each time.

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

This problem extends Problem 215 to handle a stream of numbers. The same min heap approach applies, but we maintain the heap across multiple `add` calls.

1. **Maintain Size-K Heap:** Use the same strategy as Problem 215 - maintain a min heap of size `k` that stores the `k` largest elements seen so far.

2. **Stream Processing:** Each time `add` is called:
   - If heap size < `k`, add the new value
   - Otherwise, use `heappushpop` to add the new value and remove the smallest
   - Return the root element (which is the `kth` largest)

**Key Insight:** Since Python's `heapq` is a min heap, we store elements directly. The heap automatically maintains the `k` largest elements, with the smallest of them (the `kth` largest) at the root.

**Time Complexity:** O(log k) per `add` call. Each heap operation (push/pop) takes O(log k) time. For initialization with `n` elements: O(n log k).

**Space Complexity:** O(k) for the heap.

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

## Problem: 973. K Closest Points to Origin

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

This problem finds the `k` closest points, which means we want the `k` points with the smallest distances. We use a max heap to keep track of the `k` smallest distances seen so far.

1. **Max Heap with Negative Distance:** Since Python's `heapq` is a min heap, we negate the distance to simulate a max heap. We store `[-distance, x, y]` so that points with larger distances (more negative) are at the root and can be popped.

2. **Maintain K Closest:** For each point:

   - Calculate distance squared: `x² + y²` (no need for square root for comparison)
   - Negate it: `-distance` for max heap simulation
   - If heap size < `k`, add the point
   - Otherwise, use `heappushpop` to maintain only the `k` closest points

3. **Extract Results:** Pop all elements from the heap and collect the points (distance can be discarded).

**Why Max Heap for K Closest:** We want to keep the `k` smallest distances. By using a max heap (simulated with negatives), the point with the largest distance among our `k` candidates is at the root, so we can easily remove it when a closer point is found.

**Time Complexity:** O(N log k) where N is the number of points. We process N points, and each heap operation takes O(log k) time.

**Space Complexity:** O(k) for the heap.

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

### Solution

**Logic:**

This problem requires finding the `k` most frequent elements. We need to count frequencies first, then select the top `k` by frequency.

1. **Count Frequencies:** Use `Counter` to count occurrences of each number. This takes O(N) time.

2. **Build Max Heap:** Create a heap with `(-count, num)` pairs. Negating the count simulates a max heap (since Python's heapq is min heap). We want elements with higher counts (more negative) to be at the root.

3. **Extract Top K:** Pop `k` elements from the heap. Each pop returns the element with the highest remaining frequency.

**Key Insight:** By negating frequencies, we convert a min heap into a max heap behavior. The element with the most negative count (highest frequency) will be at the root.

**Time Complexity:** O(N + U log U) where N is array length and U is number of unique elements. Counting takes O(N). Building heap from U elements takes O(U). Popping k elements takes O(k log U). Since k ≤ U, this is better than O(N log N) when U << N.

**Space Complexity:** O(U) for the frequency map and heap.

**Code:**

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

This problem extends Problem 347 to handle strings with a secondary sorting criterion. When frequencies are equal, we need lexicographical (alphabetical) order.

1. **Count Frequencies:** Count occurrences of each word using `Counter`.

2. **Heap with Custom Comparison:** Store `(-count, word)` in the heap. Python's tuple comparison works perfectly here:

   - Primary sort: by frequency (negative count, so higher frequency = more negative = higher priority)
   - Secondary sort: by word (lexicographical order, since tuples compare element by element)

3. **Extract Top K:** Pop `k` elements. The heap automatically handles ties by comparing the word string lexicographically.

**Key Insight:** Python's tuple comparison is lexicographic. When `(-count1, word1)` and `(-count2, word2)` are compared, it first compares counts, then words if counts are equal. This gives us exactly the required ordering.

**Time Complexity:** O(N + U log U) where N is the number of words and U is the number of unique words. Counting takes O(N). Heap operations take O(U log U) since we heapify U elements. Popping k elements takes O(k log U).

**Space Complexity:** O(U) for the frequency map and heap.

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
