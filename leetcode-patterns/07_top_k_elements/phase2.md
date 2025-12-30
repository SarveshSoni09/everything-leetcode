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

This problem extends the frequency-based approach from Phase 1. Instead of finding top k, we sort all characters by frequency.

1. **Count Frequencies:** Use `Counter` to count character frequencies - O(N) time.

2. **Build Max Heap:** Create a heap with `(-count, char)` pairs to simulate a max heap. Characters with higher frequencies will have more negative counts and be prioritized.

3. **Extract and Build Result:** Pop all elements from the heap and append each character repeated by its frequency count. Since we process from highest to lowest frequency, we build the result string in the desired order.

**Key Insight:** The heap naturally sorts characters by frequency. By popping all elements, we process them from highest to lowest frequency, building the sorted string efficiently.

**Time Complexity:** O(N + U log U) where N is string length and U is number of unique characters. Counting takes O(N). Heapify takes O(U). Popping U elements takes O(U log U). Building result string takes O(N).

**Space Complexity:** O(U) for the frequency map and heap, O(N) for the result string.

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

**Brute Force Approach:**
Treat the matrix as a 1D array and use the same min heap approach as Problem 215. However, this doesn't leverage the sorted property of rows and columns.

**Optimized Approach (Leveraging Sorted Property):**
Since rows and columns are sorted, we can process the matrix more efficiently:

1. **Initialize Heap:** Add the first element from each row (or first k rows) to a min heap. Store `[value, row, col]` to track position.

2. **Extract K-1 Times:** Pop the smallest element k-1 times. After each pop, add the next element from the same row (same row, next column) if it exists.

3. **Result:** The root of the heap after k-1 extractions is the kth smallest element.

**Key Insight:** Since each row is sorted, when we pop the smallest element from row `r` at column `c`, the next candidate from that row is `matrix[r][c+1]`. This way, we only need to track one position per row, using O(k) space instead of O(n²).

**Time Complexity - Brute Force:** O(n² log k) - we process n² elements, each heap operation takes O(log k).

**Time Complexity - Optimized:** O(k log min(n, k)) - we initialize heap with min(n, k) elements, then perform k-1 pops, each taking O(log min(n, k)). This is much better when k << n².

**Space Complexity - Brute Force:** O(k) for the heap.

**Space Complexity - Optimized:** O(min(n, k)) for the heap.

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

This problem requires rearranging characters so no two identical characters are adjacent. We use a greedy approach with a heap to always pick the character with the highest remaining frequency (excluding the last used character).

**Better Approach (Recommended):**

1. **Count Frequencies:** Count character frequencies using `Counter`.

2. **Build Max Heap:** Create a heap with `(-count, char)` to simulate max heap behavior.

3. **Greedy Selection:**

   - Pop the most frequent character (excluding the one we just used, stored in `prev`)
   - Append it to result and decrement its count
   - If `prev` exists (we used a character in the previous step), push it back to the heap
   - Store the current character in `prev` (so we don't use it next)
   - Continue until heap is empty

4. **Impossibility Check:** If we have a `prev` character but no heap (meaning we can't use a different character), return empty string. This happens when one character appears more than (n+1)/2 times.

**Key Insight:** By storing the last used character in `prev` and only pushing it back after selecting a different character, we ensure no two identical characters are adjacent. The greedy approach (always picking the most frequent available character) maximizes our chances of success.

**Time Complexity:** O(N log U) where N is string length and U is number of unique characters. We process each character once, and each heap operation takes O(log U) time.

**Space Complexity:** O(U) for the frequency map and heap.

**Code:**

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

**Brute Force Approach:**
Collect all values from all lists into a heap, then pop them to build the merged list. This is simple but inefficient.

**Optimized Approach (Recommended):**
Since each list is already sorted, we only need to track the current "head" of each list. The key insight is to maintain a heap of size at most `k` (number of lists), containing one element from each list.

1. **Initialize Heap:** Add the first node from each non-empty list to the heap. Store `(value, list_index, node)` to handle potential duplicate values and track which list the node belongs to.

2. **Merge Process:**

   - Pop the smallest element from the heap
   - Append it to the result list
   - If that node has a next node, add the next node from the same list to the heap

3. **Termination:** Continue until the heap is empty.

**Key Insight:** By maintaining only one active node per list in the heap, we keep the heap size at `k` instead of `N`. This reduces time complexity from O(N log N) to O(N log k), which is significantly better when k << N.

**Time Complexity - Brute Force:** O(N log N) where N is total number of nodes. We push N elements into heap, each taking O(log N).

**Time Complexity - Optimized:** O(N log k) where N is total nodes and k is number of lists. We process N nodes, and each heap operation takes O(log k) since heap size never exceeds k.

**Space Complexity - Brute Force:** O(N) for the heap.

**Space Complexity - Optimized:** O(k) for the heap.

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

**Brute Force Approach:**
Generate all pairs, push them into a heap, then pop k elements. This is inefficient when arrays are large.

**Optimized Approach (Leveraging Sorted Property):**
Since both arrays are sorted, we can process pairs more efficiently, similar to Problem 378:

1. **Initialize Heap:** Add pairs `(nums1[i], nums2[0])` for the first `min(k, len(nums1))` elements of nums1. Store `[sum, index_i, index_j]` to track positions.

2. **Extract K Pairs:**
   - Pop the pair with smallest sum
   - Add it to results
   - If we haven't exhausted nums2 for this nums1 index, add the next pair `(nums1[i], nums2[j+1])` to the heap

**Key Insight:** Since nums1 and nums2 are sorted, for each `nums1[i]`, the pairs with smallest sums will be `(nums1[i], nums2[0])`, `(nums1[i], nums2[1])`, etc. By tracking indices and only adding the next pair from the same nums1 element when we pop one, we maintain a heap of size at most `min(k, len(nums1))` instead of all n×m pairs.

**Time Complexity - Brute Force:** O(n×m log(n×m)) where n and m are array lengths. We generate all pairs and heapify them.

**Time Complexity - Optimized:** O(k log min(k, n)) where k is the number of pairs needed and n is nums1 length. We perform k heap operations, each taking O(log min(k, n)) time.

**Space Complexity - Brute Force:** O(n×m) for all pairs in the heap.

**Space Complexity - Optimized:** O(min(k, n)) for the heap.

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
