# Pattern: Slow and Fast Pointers - Phase 1

## Problem: 141. Linked List Cycle

Given head, the `head` of a linked list, determine if the linked list has a cycle in it.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to. Note that `pos` is not passed as a parameter.

Return `true` if there is a cycle in the linked list. Otherwise, return `false`.

### Example 1

```
Input: head = [3,2,0,-4], pos = 1
Output: true
Explanation: There is a cycle in the linked list, where the tail connects to the 1st node (0-indexed).
```

### Example 2

```
Input: head = [1,2], pos = 0
Output: true
Explanation: There is a cycle in the linked list, where the tail connects to the 0th node.
```

### Example 3

```
Input: head = [1], pos = -1
Output: false
Explanation: There is no cycle in the linked list.
```

### Constraints

- The number of the nodes in the list is in the range `[0, 104]`.
- `-10^5 <= Node.val <= 10^5`
- `pos` is `-1` or a valid index in the linked-list.

Follow up: Can you solve it using `O(1)` (i.e. constant) memory?

### Solution

**Logic:**

**Code:**

```python
class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        slow, fast = head, head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
            if slow == fast:
                return True
        return False
```

## Problem: 142. Linked List Cycle II

Given the `head` of a linked list, return the node where the cycle begins. If there is no cycle, return `null`.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to (0-indexed). It is `-1` if there is no cycle. Note that `pos` is not passed as a parameter.

Do not modify the linked list.

### Example 1

```
Input: head = [3,2,0,-4], pos = 1
Output: tail connects to node index 1
Explanation: There is a cycle in the linked list, where tail connects to the second node.
```

### Example 2

```
Input: head = [1,2], pos = 0
Output: tail connects to node index 0
Explanation: There is a cycle in the linked list, where tail connects to the first node.
```

### Example 3

```
Input: head = [1], pos = -1
Output: no cycle
Explanation: There is no cycle in the linked list.
```

### Constraints

- The number of the nodes in the list is in the range `[0, 10^4]`.
- `-10^5 <= Node.val <= 10^5`
- `pos` is `-1` or a valid index in the linked-list.

Follow up: Can you solve it using O(1) (i.e. constant) memory?

### Solution

**Logic:**

**Code:**

```python
class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        start, slow, fast = head, head, head
        cycle = False
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
            if slow == fast:
                cycle = True
                break

        if cycle:
            while start != fast:
                start = start.next
                fast = fast.next
            return start
        else:
            return None
```

## Problem: 287. Find the Duplicate Number

Given an array of integers `nums` containing `n + 1` integers where each integer is in the range `[1, n]` inclusive.

There is only one repeated number in `nums`, return this repeated number.

You must solve the problem without modifying the array `nums` and using only constant extra space.

### Example 1

```
Input: nums = [1,3,4,2,2]
Output: 2
```

### Example 2

```
Input: nums = [3,1,3,4,2]
Output: 3
```

### Example 3

```
Input: nums = [3,3,3,3,3]
Output: 3
```

### Constraints

- `1 <= n <= 10^5`
- `nums.length == n + 1`
- `1 <= nums[i] <= n`

All the integers in `nums` appear only once except for precisely one integer which appears two or more times.

- How can we prove that at least one duplicate number must exist in `nums`?
- Can you solve the problem in linear runtime complexity?

### Solution

**Logic:**

**Code:**

```python
class Solution:
    def findDuplicate(self, nums: List[int]) -> int:
        start = slow = fast = nums[0]
        while True:
            slow = nums[slow]
            fast = nums[nums[fast]]
            if slow == fast:
                break
        while start != fast:
            start = nums[start]
            fast = nums[fast]
        return fast
```

## Problem: 202. Happy Number

Write an algorithm to determine if a number `n` is happy.

A happy number is a number defined by the following process:

- Starting with any positive integer, replace the number by the sum of the squares of its digits.
- Repeat the process until the number equals `1` (where it will stay), or it loops endlessly in a cycle which does not include `1`.
- Those numbers for which this process ends in `1` are happy.

Return `true` if `n` is a happy number, and `false` if not.

### Example 1

```
Input: n = 19
Output: true
Explanation:
1^2 + 9^2 = 82
8^2 + 2^2 = 68
6^2 + 8^2 = 100
1^2 + 0^2 + 0^2 = 1
```

### Example 2

```
Input: n = 2
Output: false
```

### Constraints

- `1 <= n <= 2^31 - 1`

### Solution

**Logic:**

**Code:**

```python
class Solution:
    def findDuplicate(self, nums: List[int]) -> int:
        start = slow = fast = nums[0]
        while True:
            slow = nums[slow]
            fast = nums[nums[fast]]
            if slow == fast:
                break
        while start != fast:
            start = nums[start]
            fast = nums[fast]
        return fast
```

## Problem: 876. Middle of the Linked List

Given the `head` of a singly linked list, return the middle node of the linked list.

If there are two middle nodes, return the second middle node.

### Example 1

```
Input: head = [1,2,3,4,5]
Output: [3,4,5]
Explanation: The middle node of the list is node 3.
```

### Example 2

```
Input: head = [1,2,3,4,5,6]
Output: [4,5,6]
Explanation: Since the list has two middle nodes with values 3 and 4, we return the second one.
```

### Constraints

- The number of nodes in the list is in the range `[1, 100]`.
- `1 <= Node.val <= 100`

### Solution

**Logic:**

**Code:**

```python
class Solution:
    def middleNode(self, head: Optional[ListNode]) -> Optional[ListNode]:
        slow, fast = head, head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
        return slow
```

## Problem: 19. Remove Nth Node from End of List

Given the `head` of a linked list, remove the `nth` node from the end of the list and return its `head`.

### Example 1

```
Input: head = [1,2,3,4,5], n = 2
Output: [1,2,3,5]
```

### Example 2

```
Input: head = [1], n = 1
Output: []
```

### Example 3

```
Input: head = [1,2], n = 1
Output: [1]
```

### Constraints

- The number of nodes in the list is sz.
- `1 <= sz <= 30`
- `0 <= Node.val <= 100`
- `1 <= n <= sz`

**Follow up:** Could you do this in one pass?

### Solution

**Logic:**

**Code:**

```python
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        slow, fast = head, head
        while n != 0:
            if fast.next:
                fast = fast.next
            else:
                head = head.next
            n -= 1
        while fast.next:
            slow = slow.next
            fast = fast.next
        if head:
            slow.next = slow.next.next
        return head
```

## Problem: 61. Rotate List

Given the `head` of a linked list, rotate the list to the right by `k` places.

### Example 1

```
Input: head = [1,2,3,4,5], k = 2
Output: [4,5,1,2,3]
```

### Example 2

```
Input: head = [0,1,2], k = 4
Output: [2,0,1]
```

### Constraints

- The number of nodes in the list is in the range `[0, 500].`
- `-100 <= Node.val <= 100`
- `0 <= k <= 2 * 10^9`

### Solution

**Logic:**

**Code:**

```python
class Solution:
    def rotateRight(self, head: Optional[ListNode], k: int) -> Optional[ListNode]:
        if not head or k == 0:
            return head
        sz = 1
        tail = head
        while tail.next:
            tail = tail.next
            sz += 1

        k = k % sz
        if k == 0:
            return head

        r = sz - k
        tail.next = head
        new_tail = tail
        while r:
            new_tail = new_tail.next
            r -= 1

        head = new_tail.next
        new_tail.next = None
        return head
```

## Problem: 26. Remove Duplicates from Sorted Array

Given an integer array `nums` sorted in non-decreasing order, remove the duplicates in-place such that each unique element appears only once. The relative order of the elements should be kept the same.

Consider the number of unique elements in `nums` to be `k`​​​​​​​​​​​​​​. After removing duplicates, return the number of unique elements `k`.

The first `k` elements of `nums` should contain the unique numbers in sorted order. The remaining elements beyond index `k - 1` can be ignored.

### Example 1

```
Input: nums = [1,1,2]
Output: 2, nums = [1,2,_]
Explanation: Your function should return k = 2, with the first two elements of nums being 1 and 2 respectively.
It does not matter what you leave beyond the returned k (hence they are underscores).
```

### Example 2

```
Input: nums = [0,0,1,1,1,2,2,3,3,4]
Output: 5, nums = [0,1,2,3,4,_,_,_,_,_]
Explanation: Your function should return k = 5, with the first five elements of nums being 0, 1, 2, 3, and 4 respectively.
It does not matter what you leave beyond the returned k (hence they are underscores).
```

### Constraints

- `1 <= nums.length <= 3 * 10^4`
- `-100 <= nums[i] <= 100`
- `nums` is sorted in non-decreasing order.

### Solution

**Logic:**

**Code:**

```python
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        if not nums:
            return 0

        i = 0
        for j in range(1, len(nums)):
            if nums[i] != nums[j]:
                i += 1
                nums[i] = nums[j]

        return i + 1
```

## Problem: 83. Remove Duplicates from Sorted List

Given the head of a sorted linked list, delete all duplicates such that each element appears only once. Return the linked list sorted as well.

### Example 1

```
Input: head = [1,1,2]
Output: [1,2]
```

### Example 2

```
Input: head = [1,1,2,3,3]
Output: [1,2,3]
```

### Constraints

- The number of nodes in the list is in the range `[0, 300]`.
- `-100 <= Node.val <= 100`
- The list is guaranteed to be sorted in ascending order.

### Solution

**Logic:**

**Code:**

```python
class Solution:
    def deleteDuplicates(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if not head:
            return head
        slow, fast = head, head
        while fast.next:
            fast = fast.next
            if slow.val != fast.val:
                slow.next = fast
                slow = slow.next
        slow.next = None
        return head
```

## Problem: 27. Remove Element

Given an integer array `nums` and an integer `val`, remove all occurrences of `val` in `nums` in-place. The order of the elements may be changed. Then return the number of elements in `nums` which are not equal to `val`.

Consider the number of elements in `nums` which are not equal to `val` be `k`, to get accepted, you need to do the following things:

- Change the array nums such that the first `k` elements of `nums` contain the elements which are not equal to `val`. The remaining elements of `nums` are not important as well as the size of `nums`.
- Return `k`.

### Example 1

```
Input: nums = [3,2,2,3], val = 3
Output: 2, nums = [2,2,_,_]
Explanation: Your function should return k = 2, with the first two elements of nums being 2.
It does not matter what you leave beyond the returned k (hence they are underscores).
```

### Example 2

```
Input: nums = [0,1,2,2,3,0,4,2], val = 2
Output: 5, nums = [0,1,4,0,3,_,_,_]
Explanation: Your function should return k = 5, with the first five elements of nums containing 0, 0, 1, 3, and 4.
```

Note that the five elements can be returned in any order.  
It does not matter what you leave beyond the returned k (hence they are underscores).

### Constraints

- `0 <= nums.length <= 100`
- `0 <= nums[i] <= 50`
- `0 <= val <= 100`

### Solution

**Logic:**

**Code:**

```python
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        slow = 0
        for fast in range(len(nums)):
            if nums[fast] != val:
                nums[slow] = nums[fast]
                slow += 1
        return slow
```

## Problem: 160. Intersection of Two Linked Lists

Given the heads of two singly linked-lists `headA` and `headB`, return the node at which the two lists intersect. If the two linked lists have no intersection at all, return `null`.

The test cases are generated such that there are no cycles anywhere in the entire linked structure.

Note that the linked lists must retain their original structure after the function returns.

### Example 1

```
Input: intersectVal = 8, listA = [4,1,8,4,5], listB = [5,6,1,8,4,5], skipA = 2, skipB = 3
Output: Intersected at '8'
Explanation: The intersected node's value is 8 (note that this must not be 0 if the two lists intersect).
From the head of A, it reads as [4,1,8,4,5]. From the head of B, it reads as [5,6,1,8,4,5]. There are 2 nodes before the intersected node in A; There are 3 nodes before the intersected node in B.
- Note that the intersected node's value is not 1 because the nodes with value 1 in A and B (2nd node in A and 3rd node in B) are different node references. In other words, they point to two different locations in memory, while the nodes with value 8 in A and B (3rd node in A and 4th node in B) point to the same location in memory.
```

### Example 2

```
Input: intersectVal = 2, listA = [1,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1
Output: Intersected at '2'
Explanation: The intersected node's value is 2 (note that this must not be 0 if the two lists intersect).
From the head of A, it reads as [1,9,1,2,4]. From the head of B, it reads as [3,2,4]. There are 3 nodes before the intersected node in A; There are 1 node before the intersected node in B.
```

### Example 3

```
Input: intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2
Output: No intersection
Explanation: From the head of A, it reads as [2,6,4]. From the head of B, it reads as [1,5]. Since the two lists do not intersect, intersectVal must be 0, while skipA and skipB can be arbitrary values.
Explanation: The two lists do not intersect, so return null.
```

### Constraints

- The number of nodes of `listA` is in the `m`.
- The number of nodes of `listB` is in the `n`.
- `1 <= m, n <= 3 * 104`
- `1 <= Node.val <= 105`
- `0 <= skipA <= m`
- `0 <= skipB <= n`
- `intersectVal` is `0` if `listA` and `listB` do not intersect.
- `intersectVal == listA[skipA] == listB[skipB]` if `listA` and `listB` intersect.

Follow up: Could you write a solution that runs in O(m + n) time and use only O(1) memory?

### Solution

**Logic:**

**Code:**

```python
class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> Optional[ListNode]:
        fast = headB
        slow = headA
        while slow != fast:
            slow = slow.next if slow else headB
            fast = fast.next if fast else headA
        return slow
```

## Problem: 234. Palindrome List

Given the `head` of a singly linked list, return `true` if it is a palindrome or `false` otherwise.

### Example 1

```
Input: head = [1,2,2,1]
Output: true
```

### Example 2

```
Input: head = [1,2]
Output: false
```

### Constraints

- The number of nodes in the list is in the range `[1, 105]`.
- `0 <= Node.val <= 9`

### Solution

**Logic:**

**Code:**

```python
class Solution:
    def isPalindrome(self, head: Optional[ListNode]) -> bool:
        slow, fast = head, head
        while fast and fast.next:
            fast = fast.next.next
            slow = slow.next

        prev = None
        while slow:
            temp = slow.next
            slow.next = prev
            prev = slow
            slow = temp

        left, right = head, prev
        while right:
            if left.val != right.val:
                return False
            left = left.next
            right = right.next
        return True
```
