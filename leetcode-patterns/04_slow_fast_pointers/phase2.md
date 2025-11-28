# Pattern: Slow and Fast Pointers - Phase 2

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

This problem is a brilliant application of the cycle detection pattern from Phase 1. The key insight is to treat the array as a linked list where `nums[i]` points to `nums[nums[i]]`. The duplicate number creates a cycle in this "linked list."

1. **Array as Linked List:** Treat the array as a linked list:

   - Each index `i` represents a node
   - The value `nums[i]` represents the "next" pointer (pointing to index `nums[i]`)
   - Since values are in range `[1, n]` and we have `n+1` elements, the duplicate creates a cycle

2. **Cycle Detection:** Use the slow/fast pointer technique from Problem 141:

   - Start both pointers at `nums[0]` (the first "node")
   - `slow = nums[slow]` (move one step)
   - `fast = nums[nums[fast]]` (move two steps)
   - When they meet, we've detected a cycle

3. **Finding the Duplicate:** After detecting the cycle, use the technique from Problem 142:

   - Keep `fast` at the meeting point
   - Start `start` from `nums[0]`
   - Move both one step at a time: `start = nums[start]`, `fast = nums[fast]`
   - They will meet at the duplicate number (the cycle start)

4. **Why This Works:** The duplicate number is the entry point to the cycle. The mathematical proof from Problem 142 applies here: the distance from the start to the cycle entry equals the distance from the meeting point to the cycle entry.

5. **Key Insight:** This demonstrates that slow/fast pointers work on **implicit linked structures** - we don't need actual linked list nodes, just a way to define "next" operations.

6. **Time Complexity:** O(N) - we traverse the array at most twice.
7. **Space Complexity:** O(1) - only a few variables are used.

This problem shows how to **apply cycle detection to arrays** by treating them as implicit linked lists.

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

---

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

This problem introduces the **two-pointer technique for in-place array modification**. We use a slow pointer to track where to place unique elements and a fast pointer to scan through the array.

1. **Two-Pointer Setup:**

   - `i` (slow pointer): tracks the position where the next unique element should be placed
   - `j` (fast pointer): scans through the array to find unique elements

2. **The Strategy:**

   - Start with `i = 0` (first position is always unique)
   - For each `j` from `1` to `len(nums)-1`:
     - If `nums[j] != nums[i]`, we found a new unique element
     - Increment `i` and place the new unique element: `nums[i] = nums[j]`
   - Continue until `j` reaches the end

3. **Why This Works:** Since the array is sorted, duplicates are adjacent. When we find `nums[j] != nums[i]`, we know `nums[j]` is a new unique element. By placing it at position `i+1`, we maintain the sorted order of unique elements.

4. **Key Insight:** The slow pointer (`i`) maintains the "write position" for unique elements, while the fast pointer (`j`) finds the next unique element. This is a fundamental pattern for in-place array modifications.

5. **Time Complexity:** O(N) - we traverse the array once.
6. **Space Complexity:** O(1) - only two pointers are used, no extra space.

This problem establishes the **two-pointer pattern for in-place array filtering** - a crucial technique that will be extended in subsequent problems.

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

---

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

This problem applies the same two-pointer technique from Problem 26 to a linked list. The slow pointer maintains the last unique node, while the fast pointer scans ahead to find the next unique value.

1. **Two-Pointer Setup:**

   - `slow`: points to the last unique node we've kept
   - `fast`: scans ahead to find nodes with different values

2. **The Strategy:**

   - Start with `slow = head` and `fast = head`
   - While `fast.next` exists:
     - Move `fast` forward: `fast = fast.next`
     - If `slow.val != fast.val`, we found a new unique value:
       - Link `slow.next` to `fast`: `slow.next = fast`
       - Move `slow` forward: `slow = slow.next`
   - After the loop, set `slow.next = None` to terminate the list

3. **Why This Works:** Since the list is sorted, duplicates are adjacent. When `slow.val != fast.val`, we've found the next unique node. By linking `slow.next` to `fast`, we skip all duplicate nodes between them.

4. **Key Insight:** This is the linked list version of Problem 26. The same two-pointer pattern applies, but instead of overwriting array elements, we modify `next` pointers to skip duplicate nodes.

5. **Time Complexity:** O(N) - we traverse the list once.
6. **Space Complexity:** O(1) - only two pointers are used.

This problem shows that the **two-pointer in-place modification pattern** works for both arrays and linked lists.

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

---

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

This problem extends the two-pointer pattern from Problem 26 to **filter out specific values**. The slow pointer tracks where to place valid elements, while the fast pointer scans for elements that should be kept.

1. **Two-Pointer Setup:**

   - `slow`: tracks the position where the next valid (non-`val`) element should be placed
   - `fast`: scans through the array to find elements that are not equal to `val`

2. **The Strategy:**

   - Start with `slow = 0`
   - For each `fast` from `0` to `len(nums)-1`:
     - If `nums[fast] != val`, this is a valid element to keep
     - Place it at position `slow`: `nums[slow] = nums[fast]`
     - Increment `slow` to prepare for the next valid element
   - Return `slow` as the count of valid elements

3. **Why This Works:** The slow pointer maintains the "write position" for valid elements. Every time we find a valid element (not equal to `val`), we write it to the slow pointer's position and advance slow. This effectively filters out all occurrences of `val`.

4. **Key Insight:** This is a generalization of Problem 26. Instead of removing duplicates (comparing adjacent elements), we're filtering based on a condition (comparing with `val`). The same two-pointer pattern applies.

5. **Time Complexity:** O(N) - we traverse the array once.
6. **Space Complexity:** O(1) - only two pointers are used.

This problem demonstrates the **two-pointer filtering pattern** - a versatile technique for in-place array modifications based on any condition.

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

---

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

This problem combines multiple two-pointer techniques: finding the list length, finding the kth node from the end (similar to Problem 19), and manipulating pointers to rotate the list.

1. **Edge Cases:** Handle empty list or `k == 0` by returning `head` immediately.

2. **Find List Length:** Traverse the list to find its length `sz`. This is necessary because `k` might be larger than the list length.

3. **Normalize k:** Since rotating by `k` is the same as rotating by `k % sz`, we reduce `k` to be less than the list length. If `k == 0` after normalization, return `head`.

4. **Find Rotation Point:** We need to find the node that will become the new tail. This is the `(sz - k)`th node from the beginning:

   - Start from `head` and move `(sz - k)` steps
   - This node's `next` will become the new `head`
   - This node itself will become the new tail

5. **Perform Rotation:**

   - Connect the original tail to the original head: `tail.next = head`
   - Set the new head: `head = new_tail.next`
   - Break the cycle: `new_tail.next = None`

6. **Why This Works:** Rotating right by `k` means moving the last `k` nodes to the front. The node at position `sz - k` from the start is where we need to split the list.

7. **Key Insight:** This problem combines:

   - Finding list length (single pointer traversal)
   - Finding a specific position from the start (pointer movement)
   - Pointer manipulation to restructure the list

8. **Time Complexity:** O(N) - we traverse the list twice: once to find length, once to find the rotation point.
9. **Space Complexity:** O(1) - only a few pointers are used.

This problem demonstrates how to **combine multiple two-pointer techniques** to solve complex linked list manipulation problems.

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
