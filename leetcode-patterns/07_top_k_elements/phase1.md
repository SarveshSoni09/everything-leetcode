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

## Problem: 355. Design Twitter

Design a simplified version of Twitter where users can post tweets, follow/unfollow another user, and is able to see the `10` most recent tweets in the user's news feed.

Implement the `Twitter` class:

- `Twitter()` Initializes your twitter object.
- `void postTweet(int userId, int tweetId)` Composes a new tweet with ID `tweetId` by the user `userId`. Each call to this function will be made with a unique `tweetId`.
- `List<Integer> getNewsFeed(int userId)` Retrieves the `10` most recent tweet IDs in the user's news feed. Each item in the news feed must be posted by users who the user followed or by the user themself. Tweets must be ordered from most recent to least recent.
- `void follow(int followerId, int followeeId)` The user with ID `followerId` started following the user with ID `followeeId`.
- `void unfollow(int followerId, int followeeId)` The user with ID `followerId` started unfollowing the user with ID `followeeId`.

### Example 1

```
Input
["Twitter", "postTweet", "getNewsFeed", "follow", "postTweet", "getNewsFeed", "unfollow", "getNewsFeed"]
[[], [1, 5], [1], [1, 2], [2, 6], [1], [1, 2], [1]]
Output
[null, null, [5], null, null, [6, 5], null, [5]]

Explanation
Twitter twitter = new Twitter();
twitter.postTweet(1, 5); // User 1 posts a new tweet (id = 5).
twitter.getNewsFeed(1);  // User 1's news feed should return a list with 1 tweet id -> [5]. return [5]
twitter.follow(1, 2);    // User 1 follows user 2.
twitter.postTweet(2, 6); // User 2 posts a new tweet (id = 6).
twitter.getNewsFeed(1);  // User 1's news feed should return a list with 2 tweet ids -> [6, 5]. Tweet id 6 should precede tweet id 5 because it is posted after tweet id 5.
twitter.unfollow(1, 2);  // User 1 unfollows user 2.
twitter.getNewsFeed(1);  // User 1's news feed should return a list with 1 tweet id -> [5], since user 1 is no longer following user 2.
```

### Constraints

- `1 <= userId, followerId, followeeId <= 500`
- `0 <= tweetId <= 10^4`
- All the tweets have unique IDs.
- At most `3 * 10^4` calls will be made to `postTweet`, `getNewsFeed`, `follow`, and `unfollow`.
- A user cannot follow himself.

### Solution

**Logic:**

**Code:**

```python
class Twitter:

    def __init__(self):
        self.order = 0
        self.tweetMap = defaultdict(list)
        self.followMap = defaultdict(set)

    def postTweet(self, userId: int, tweetId: int) -> None:
        self.tweetMap[userId].append([self.order, tweetId])
        self.order -= 1

    def getNewsFeed(self, userId: int) -> List[int]:
        res = []
        minHeap = []

        self.followMap[userId].add(userId)
        for followeeId in self.followMap[userId]:
            if followeeId in self.tweetMap:
                index = len(self.tweetMap[followeeId]) - 1
                count, tweetId = self.tweetMap[followeeId][index]
                minHeap.append([count, tweetId, followeeId, index - 1])
        heapq.heapify(minHeap)
        while minHeap and len(res) < 10:
            count, tweetId, followeeId, index = heapq.heappop(minHeap)
            res.append(tweetId)
            if index >= 0:
                count, tweetId = self.tweetMap[followeeId][index]
                heapq.heappush(minHeap, [count, tweetId, followeeId, index - 1])
        return res

    def follow(self, followerId: int, followeeId: int) -> None:
        self.followMap[followerId].add(followeeId)

    def unfollow(self, followerId: int, followeeId: int) -> None:
        if followeeId in self.followMap[followerId]:
            self.followMap[followerId].remove(followeeId)
```

## Problem: 1094. Car Pooling

There is a car with `capacity` empty seats. The vehicle only drives east (i.e., it cannot turn around and drive west).

You are given the integer `capacity` and an array `trips` where `trips[i] = [numPassengersi, fromi, toi]` indicates that the `ith` trip has `numPassengersi` passengers and the locations to pick them up and drop them off are `fromi` and `toi` respectively. The locations are given as the number of kilometers due east from the car's initial location.

Return `true` if it is possible to pick up and drop off all passengers for all the given trips, or `false` otherwise.

### Example 1

```
Input: trips = [[2,1,5],[3,3,7]], capacity = 4
Output: false
```

### Example 2

```
Input: trips = [[2,1,5],[3,3,7]], capacity = 5
Output: true
```

### Constraints

- `1 <= trips.length <= 1000`
- `trips[i].length == 3`
- `1 <= numPassengersi <= 100`
- `0 <= fromi < toi <= 1000`
- `1 <= capacity <= 105`

### Solution

**Logic:**

**Code:**

```python
import heapq
class Solution:
    def carPooling(self, trips: List[List[int]], capacity: int) -> bool:
        start_heap = []
        end_heap = []
        cap_avail = capacity
        for px, start, end in trips:
            heapq.heappush(start_heap, [start, end, px])
            heapq.heappush(end_heap, [end, px])
        while start_heap:
            start, end, px = heapq.heappop(start_heap)
            cap_avail -= px
            if cap_avail < 0:
                while end_heap and end_heap[0][0] <= start:
                    e, cap = heapq.heappop(end_heap)
                    cap_avail += cap
                if cap_avail < 0:
                    return False
        return True
```

## Problem: 1705. Maximum Number of Eaten Apples

There is a special kind of apple tree that grows apples every day for `n` days. On the `ith` day, the tree grows `apples[i]` apples that will rot after `days[i]` days, that is on day `i + days[i]` the apples will be rotten and cannot be eaten. On some days, the apple tree does not grow any apples, which are denoted by `apples[i] == 0` and `days[i] == 0`.

You decided to eat at most one apple a day (to keep the doctors away). Note that you can keep eating after the first `n` days.

Given two integer arrays days and apples of length `n`, return the maximum number of apples you can eat.

### Example 1

```
Input: apples = [1,2,3,5,2], days = [3,2,1,4,2]
Output: 7
Explanation: You can eat 7 apples:
- On the first day, you eat an apple that grew on the first day.
- On the second day, you eat an apple that grew on the second day.
- On the third day, you eat an apple that grew on the second day. After this day, the apples that grew on the third day rot.
- On the fourth to the seventh days, you eat apples that grew on the fourth day.
```

### Example 2

```
Input: apples = [3,0,0,0,0,2], days = [3,0,0,0,0,2]
Output: 5
Explanation: You can eat 5 apples:
- On the first to the third day you eat apples that grew on the first day.
- Do nothing on the fouth and fifth days.
- On the sixth and seventh days you eat apples that grew on the sixth day.
```

### Constraints

- `n == apples.length == days.length`
- `1 <= n <= 2 * 10^4`
- `0 <= apples[i], days[i] <= 2 * 10^4`
- `days[i] = 0` if and only if `apples[i] = 0.`

### Solution

**Logic:**

**Code:**

Brute force Solution:

```python
import heapq
class Solution:
    def eatenApples(self, apples: List[int], days: List[int]) -> int:
        heap = []
        res = 0
        for i in range(len(apples)):
            apple = apples[i]
            day = days[i]
            if apple and day:
                heapq.heappush(heap, [day, apple])
            if heap:
                res += 1
                eat_day, eat_apple = heapq.heappop(heap)
                eat_apple -= 1
                eat_day -= 1
                temp_heap = []
                while heap:
                    d, a = heapq.heappop(heap)
                    d -= 1
                    if d:
                        heapq.heappush(temp_heap, [d, a])
                if eat_day and eat_apple:
                    heapq.heappush(temp_heap, [eat_day, eat_apple])
                heap = temp_heap
        while heap:
            res += 1
            eat_day, eat_apple = heapq.heappop(heap)
            eat_apple -= 1
            eat_day -= 1
            temp_heap = []
            while heap:
                d, a = heapq.heappop(heap)
                d -= 1
                if d:
                    heapq.heappush(temp_heap, [d, a])
            if eat_day and eat_apple:
                heapq.heappush(temp_heap, [eat_day, eat_apple])
            heap = temp_heap

        return res
```

Optimized Solution:

```python
import heapq
class Solution:
    def eatenApples(self, apples: List[int], days: List[int]) -> int:
        heap = []
        res, i = 0, 0
        n = len(apples)
        while i < n or heap:
            if i < n and apples[i] > 0:
                heapq.heappush(heap, [i + days[i], apples[i]])
            while heap and heap[0][0] <= i:
                heapq.heappop(heap)
            if heap:
                heap[0][1] -= 1
                res += 1
                if not heap[0][1]:
                    heapq.heappop(heap)
            i += 1
        return res
```

## Problem: 295. Find Median from Data Stream

The median is the middle value in an ordered integer list. If the size of the list is even, there is no middle value, and the median is the mean of the two middle values.

- For example, for `arr = [2,3,4]`, the median is `3`.
- For example, for `arr = [2,3]`, the median is `(2 + 3) / 2 = 2.5`.

Implement the MedianFinder class:

- `MedianFinder()` initializes the `MedianFinder` object.
- `void addNum(int num)` adds the integer `num` from the data stream to the data structure.
- `double findMedian()` returns the median of all elements so far. Answers within `10^-5` of the actual answer will be accepted.

### Example 1

```
Input
["MedianFinder", "addNum", "addNum", "findMedian", "addNum", "findMedian"]
[[], [1], [2], [], [3], []]
Output
[null, null, null, 1.5, null, 2.0]

Explanation
MedianFinder medianFinder = new MedianFinder();
medianFinder.addNum(1);    // arr = [1]
medianFinder.addNum(2);    // arr = [1, 2]
medianFinder.findMedian(); // return 1.5 (i.e., (1 + 2) / 2)
medianFinder.addNum(3);    // arr[1, 2, 3]
medianFinder.findMedian(); // return 2.0
```

### Constraints

- `-10^5 <= num <= 10^5`
- There will be at least one element in the data structure before calling `findMedian`.
- At most `5 * 10^4` calls will be made to `addNum` and `findMedian`.

### Solution

**Logic:**

**Code:**

Brute force Solution

```python
import heapq
class MedianFinder:

    def __init__(self):
        self.arr = []
        self.count = -1

    def addNum(self, num: int) -> None:
        self.arr.append(num)
        self.count += 1

    def findMedian(self) -> float:
        temp_arr = []
        heapq.heapify(self.arr)
        if self.count % 2 == 0:
            n = self.count // 2
            while n >= 0:
                val = heapq.heappop(self.arr)
                temp_arr.append(val)
                n -= 1
            self.arr = self.arr + temp_arr
            return val
        else:
            n = self.count // 2
            while n >= 0:
                val = heapq.heappop(self.arr)
                temp_arr.append(val)
                n -= 1
            val2 = heapq.heappop(self.arr)
            temp_arr.append(val2)
            self.arr = self.arr + temp_arr
            return (val + val2) / 2
```

Optimized Solution

```python
import heapq
class MedianFinder:

    def __init__(self):
        self.small_heap = []
        self.large_heap = []

    def addNum(self, num: int) -> None:
        if self.large_heap and num >= self.large_heap[0]:
            heapq.heappush(self.large_heap, num)
        else:
            heapq.heappush(self.small_heap, -num)
        if len(self.large_heap) - len(self.small_heap) > 1:
            heapq.heappush(self.small_heap, -heapq.heappop(self.large_heap))
        if len(self.small_heap) - len(self.large_heap) > 1:
            heapq.heappush(self.large_heap, -heapq.heappop(self.small_heap))


    def findMedian(self) -> float:
        if len(self.small_heap) == len(self.large_heap):
            return (-self.small_heap[0] + self.large_heap[0])/2
        elif len(self.small_heap) > len(self.large_heap):
            return -self.small_heap[0]
        else:
            return self.large_heap[0]
```
