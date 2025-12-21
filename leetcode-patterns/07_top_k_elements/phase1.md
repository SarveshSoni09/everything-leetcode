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

## Problem: 767. Reorganize the String

Given a string `s`, rearrange the characters of `s` so that any two adjacent characters are not the same.

Return any possible rearrangement of `s` or return `""` if not possible.

### Example 1

```
Input: s = "aab"
Output: "aba"
```

### Example 2

```
Input: s = "aaab"
Output: ""
```

### Constraints

- `1 <= s.length <= 500`
- `s` consists of lowercase English letters.

### Solution

**Logic:**

**Code:**

```python
import heapq
class Solution:
    def reorganizeString(self, s: str) -> str:
        char_freq = Counter(s)
        max_heap = [(-count, char) for char, count in char_freq.items()]
        heapq.heapify(max_heap)
        res = []
        while max_heap:
            count, char = heapq.heappop(max_heap)
            if res and res[-1] == char:
                if max_heap:
                    ncount, nchar = count, char
                    count, char = heapq.heappop(max_heap)
                    heapq.heappush(max_heap, (ncount, nchar))
                else:
                    return ""
            res.append(char)
            count += 1
            if count:
                heapq.heappush(max_heap, (count, char))
        return ''.join(res)
```

Or a Better Way

```python
import heapq
class Solution:
    def reorganizeString(self, s: str) -> str:

        char_freq = Counter(s)
        max_heap = [(-count, char) for char, count in char_freq.items()]
        heapq.heapify(max_heap)

        prev = None
        res = ''
        while max_heap or prev:
            if prev and not max_heap:
                return ""
            count, char = heapq.heappop(max_heap)
            res += char
            count += 1
            if prev:
                heapq.heappush(max_heap, prev)
                prev = None
            if count != 0:
                prev = (count, char)
        return res
```

## Problem: 659. Split Array into Consecutive Subsequences

You are given an integer array `nums` that is sorted in non-decreasing order.

Determine if it is possible to split `nums` into one or more subsequences such that both of the following conditions are true:

- Each subsequence is a consecutive increasing sequence (i.e. each integer is exactly one more than the previous integer).
- All subsequences have a length of 3 or more.

Return `true` if you can split nums according to the above conditions, or `false` otherwise.

A subsequence of an array is a new array that is formed from the original array by deleting some (can be none) of the elements without disturbing the relative positions of the remaining elements. (i.e., [1,3,5] is a subsequence of [1,2,3,4,5] while [1,3,2] is not).

### Example 1

```
Input: nums = [1,2,3,3,4,5]
Output: true
Explanation: nums can be split into the following subsequences:
[1,2,3,3,4,5] --> 1, 2, 3
[1,2,3,3,4,5] --> 3, 4, 5
```

### Example 2

```
Input: nums = [1,2,3,3,4,4,5,5]
Output: true
Explanation: nums can be split into the following subsequences:
[1,2,3,3,4,4,5,5] --> 1, 2, 3, 4, 5
[1,2,3,3,4,4,5,5] --> 3, 4, 5
```

### Example 3

```
Input: nums = [1,2,3,4,4,5]
Output: false
Explanation: It is impossible to split nums into consecutive increasing subsequences of length 3 or more.
```

### Constraints

- `1 <= nums.length <= 10^4`
- `-1000 <= nums[i] <= 1000`
- `nums` is sorted in non-decreasing order.

### Solution

**Logic:**

**Code:**

```python
import heapq
class Solution:
    def isPossible(self, nums: List[int]) -> bool:
        sub_seqs = defaultdict(list)
        for num in nums:
            if sub_seqs[num - 1]:
                prev_len = heapq.heappop(sub_seqs[num - 1])
                heapq.heappush(sub_seqs[num], prev_len + 1)
            else:
                heapq.heappush(sub_seqs[num], 1)
        for lens in sub_seqs.values():
            for l in lens:
                if l < 3:
                    return False
        return True
```

## Problem: 23. Merge k Sorted Lists

You are given an array of `k` linked-lists `lists`, each linked-list is sorted in ascending order.

Merge all the linked-lists into one sorted linked-list and return it.

### Example 1

```
Input: lists = [[1,4,5],[1,3,4],[2,6]]
Output: [1,1,2,3,4,4,5,6]
Explanation: The linked-lists are:
[
  1->4->5,
  1->3->4,
  2->6
]
merging them into one sorted linked list:
1->1->2->3->4->4->5->6
```

### Example 2

```
Input: lists = []
Output: []
```

### Example 3

```
Input: lists = [[]]
Output: []
```

### Constraints

- `k == lists.length`
- `0 <= k <= 10^4`
- `0 <= lists[i].length <= 500`
- `-10^4 <= lists[i][j] <= 10^4`
- `lists[i]` is sorted in ascending order.
- The sum of `lists[i].length` will not exceed `10^4`

### Solution

**Logic:**

**Code:**

The following solution has a time complexity of $O(N\log N)$ and space complexity of $O(N)$.  
This can be optimized down to $O(N\log k)$ time complexity and $O(k)$ memory.

```python
import heapq
class Solution:
    def mergeKLists(self, lists: List[Optional[ListNode]]) -> Optional[ListNode]:
        heap = []
        for ll in lists:
            while ll:
                heapq.heappush(heap, ll.val)
                ll = ll.next
        head = ListNode(0)
        curr = head
        while heap:
            curr.next = ListNode(heapq.heappop(heap))
            curr = curr.next
        return head.next
```

In the above code, we pushed every single element into the heap and thus the memory is $O(N)$.  
The time complexity can be dropped if we limited the size of the heap to just $k$ elements (one active node per list) instead of loading all $N$ elements at once.  
Since the cost of a heap operation depends on the heap's size, paying $O(\log k)$ for each of the $N$ steps is significantly cheaper than paying $O(\log N)$

```python
import heapq
class Solution:
    def mergeKLists(self, lists: List[Optional[ListNode]]) -> Optional[ListNode]:
        min_heap = []
        for i, l in enumerate(lists):
            if l:
                heapq.heappush(min_heap, (l.val, i, l))
        head = ListNode(0)
        curr = head
        while min_heap:
            val, i, node = heapq.heappop(min_heap)
            curr.next = node
            curr = curr.next
            if node.next:
                heapq.heappush(min_heap, (node.next.val, i, node.next))
        return head.next
```
