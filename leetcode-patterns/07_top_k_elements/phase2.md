# Pattern: Top K Elements - Phase 2

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



## Problem: 378. Kth Smallest Element in a Sorted Matrix

Given an `n x n` `matrix` where each of the rows and columns is sorted in ascending order, return the `kth` smallest element in the matrix.

Note that it is the `kth` smallest element in the sorted order, not the `kth` distinct element.

You must find a solution with a memory complexity better than `O(n2)`

### Example 1

```
Input: matrix = [[1,5,9],[10,11,13],[12,13,15]], k = 8
Output: 13
Explanation: The elements in the matrix are [1,5,9,10,11,12,13,13,15], and the 8th smallest number is 13
```

### Example 2

```
Input: matrix = [[-5]], k = 1
Output: -5
```

### Constraints

- `n == matrix.length == matrix[i].length`
- `1 <= n <= 300`
- `-10^9 <= matrix[i][j] <= 10^9`
- All the rows and columns of matrix are guaranteed to be sorted in non-decreasing order.
- `1 <= k <= n^2`

Follow up:

Could you solve the problem with a constant memory (i.e., O(1) memory complexity)?

### Solution

**Logic:**

**Code:**

Brute force Solution

```python
import heapq
class Solution:
    def kthSmallest(self, matrix: List[List[int]], k: int) -> int:
        n = len(matrix)
        if n == 1:
            return matrix[0][0]
        max_heap = []
        for r in range(n):
            for c in range(n):
                if len(max_heap) < k:
                    heapq.heappush(max_heap, -matrix[r][c])
                else:
                    if -matrix[r][c] > max_heap[0]:
                        heapq.heappushpop(max_heap, -matrix[r][c])
                    else:
                        break
        return -max_heap[0]
```

Optimized Solution

```python
import heapq
class Solution:
    def kthSmallest(self, matrix: List[List[int]], k: int) -> int:
        n = len(matrix)
        min_heap = []
        for r in range(min(n, k)):
            heapq.heappush(min_heap, [matrix[r][0], r, 0])
        for _ in range(k-1):
            val, r, c = heapq.heappop(min_heap)
            if c + 1 < n:
                heapq.heappush(min_heap, [matrix[r][c+1], r, c+1])
        return min_heap[0][0]
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



## Problem: 373. Find K Pairs with Smallest Sums

You are given two integer arrays `nums1` and `nums2` sorted in non-decreasing order and an integer `k`.

Define a pair `(u, v)` which consists of one element from the first array and one element from the second array.

Return the `k` pairs `(u1, v1), (u2, v2), ..., (uk, vk)` with the smallest sums.

### Example 1

```
Input: nums1 = [1,7,11], nums2 = [2,4,6], k = 3
Output: [[1,2],[1,4],[1,6]]
Explanation: The first 3 pairs are returned from the sequence: [1,2],[1,4],[1,6],[7,2],[7,4],[11,2],[7,6],[11,4],[11,6]
```

### Example 2

```
Input: nums1 = [1,1,2], nums2 = [1,2,3], k = 2
Output: [[1,1],[1,1]]
Explanation: The first 2 pairs are returned from the sequence: [1,1],[1,1],[1,2],[2,1],[1,2],[2,2],[1,3],[1,3],[2,3]
```

### Constraints

- `1 <= nums1.length, nums2.length <= 10^5`
- `-10^9 <= nums1[i], nums2[i] <= 10^9`
- `nums1` and `nums2` both are sorted in non-decreasing order.
- `1 <= k <= 10^4`
- `k <= nums1.length * nums2.length`

### Solution

**Logic:**

**Code:**

Brute force Solution:

```python
import heapq
class Solution:
    def kSmallestPairs(self, nums1: List[int], nums2: List[int], k: int) -> List[List[int]]:
        heap = []
        for n1 in nums1:
            for n2 in nums2:
                small_sum = n1 + n2
                heapq.heappush(heap, [small_sum, n1, n2])
        res = []
        while k:
            ss, u, v = heapq.heappop(heap)
            res.append([u, v])
            k -= 1
        return res
```

Optimized Solution:

```python
import heapq
class Solution:
    def kSmallestPairs(self, nums1: List[int], nums2: List[int], k: int) -> List[List[int]]:
        min_heap = []
        for i in range(min(k, len(nums1))):
            curr_sum = nums1[i] + nums2[0]
            heapq.heappush(min_heap, [curr_sum, i, 0])
        res = []
        while min_heap and len(res) < k:
            curr_sum, i, j = heapq.heappop(min_heap)
            res.append([nums1[i], nums2[j]])
            if j+1 < len(nums2):
                curr_sum = nums1[i] + nums2[j+1]
                heapq.heappush(min_heap, [curr_sum, i, j+1])
        return res
```