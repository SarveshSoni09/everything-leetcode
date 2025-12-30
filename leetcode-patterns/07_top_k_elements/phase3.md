# Pattern: Top K Elements - Phase 3

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