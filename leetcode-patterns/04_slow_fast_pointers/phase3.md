# Pattern: Slow and Fast Pointers - Phase 3

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

This problem uses a clever two-pointer technique that handles different list lengths by "switching" pointers between the two lists. The key insight is that if we traverse both lists and switch to the other list when we reach the end, both pointers will eventually meet at the intersection point (or both become `None` if there's no intersection).

1. **The Two-Pointer Setup:**

   - `slow`: starts at `headA`
   - `fast`: starts at `headB`

2. **The Movement Strategy:**

   - Move both pointers one step at a time: `slow = slow.next`, `fast = fast.next`
   - When `slow` reaches the end of list A, switch it to `headB`
   - When `fast` reaches the end of list B, switch it to `headA`
   - Continue until `slow == fast` (intersection found) or both are `None` (no intersection)

3. **Why This Works:**

   - If the lists have the same length and intersect, the pointers will meet at the intersection.
   - If the lists have different lengths, switching pointers compensates for the length difference:
     - After `slow` traverses list A (length `m`) and switches to list B, it will be `m` steps behind the intersection
     - After `fast` traverses list B (length `n`) and switches to list A, it will be `n` steps behind the intersection
     - Since both pointers now traverse the same "combined" path, they will meet at the intersection

4. **Mathematical Proof:** Let `a` be the length before intersection in list A, `b` be the length before intersection in list B, and `c` be the length of the common part. After switching:

   - `slow` has traveled: `a + c + b` steps
   - `fast` has traveled: `b + c + a` steps
   - They've traveled the same distance and will meet at the intersection.

5. **Key Insight:** This demonstrates that two pointers can work on **multiple structures** by switching between them, effectively creating a "virtual" combined structure.

6. **Time Complexity:** O(m + n) - each pointer traverses at most both lists once.
7. **Space Complexity:** O(1) - only two pointers are used.

This problem shows how to use **two pointers on multiple linked structures** by cleverly switching between them.

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

---

## Problem: 234. Palindrome Linked List

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

This problem combines multiple techniques: using slow/fast pointers to find the middle (from Problem 876), reversing a linked list, and comparing two halves. The key insight is to reverse the second half of the list and compare it with the first half.

1. **Find the Middle:** Use slow/fast pointers to find the middle of the list:

   - `slow` moves one step, `fast` moves two steps
   - When `fast` reaches the end, `slow` is at the middle (or just past it for even-length lists)

2. **Reverse the Second Half:** Starting from `slow`, reverse the second half of the list:

   - Use three pointers: `prev`, `slow` (current), and `temp` (next)
   - Standard linked list reversal technique

3. **Compare the Two Halves:**

   - `left` starts at `head` (first half)
   - `right` starts at `prev` (reversed second half, which is the last node of the original list)
   - Compare values while moving both pointers forward
   - If all values match, it's a palindrome

4. **Why This Works:**

   - By reversing the second half, we can traverse it from end to middle
   - Comparing the first half (forward) with the reversed second half (backward) checks if the list reads the same in both directions
   - This avoids needing to store the entire list or use O(n) extra space

5. **Key Insight:** This problem demonstrates combining multiple two-pointer techniques:

   - Slow/fast pointers to find the middle
   - Pointer manipulation to reverse a linked list
   - Two pointers to compare two halves

6. **Time Complexity:** O(N) - we traverse the list three times: once to find the middle, once to reverse, once to compare.
7. **Space Complexity:** O(1) - only a few pointers are used, the reversal is done in-place.

This problem represents a **synthesis of multiple two-pointer techniques** to solve a complex problem efficiently.

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

---

## Problem: 448. Find All Numbers Disappeared in an Array

Given an array nums of `n` integers where `nums[i]` is in the range `[1, n]`, return an array of all the integers in the range `[1, n]` that do not appear in `nums`.

### Example 1

```
Input: nums = [4,3,2,7,8,2,3,1]
Output: [5,6]
```

### Example 2

```
Input: nums = [1,1]
Output: [2]
```

### Constraints

- `n == nums.length`
- `1 <= n <= 10^5`
- `1 <= nums[i] <= n`

Follow up: Could you do it without extra space and in O(n) runtime? You may assume the returned list does not count as extra space.

### Solution

**Logic:**

This problem uses a clever array indexing pattern. Since all values are in the range `[1, n]` and we have `n` elements, we can use the array itself as a hash table by marking indices as "seen" using negative signs.

1. **The Marking Strategy:** For each number `n` in the array:

   - Calculate the index it should mark: `i = abs(n) - 1`
   - Mark that index as "seen" by making it negative: `nums[i] = -abs(nums[i])`
   - Use `abs()` to handle cases where the value is already negative

2. **Why This Works:**

   - Each number `n` in the array should mark position `n-1` (since arrays are 0-indexed)
   - If a number from `[1, n]` is missing, its corresponding index will never be marked (will remain positive)
   - After marking, we can scan the array to find all positive values, which indicate missing numbers

3. **Finding Missing Numbers:** After marking:

   - Iterate through the array
   - If `nums[i] > 0`, it means index `i+1` was never marked, so `i+1` is a missing number
   - Add `i+1` to the result list

4. **Key Insight:** This demonstrates using **array indices as hash keys** and the array values as markers. This is a powerful technique when:

   - Values are in a known range `[1, n]`
   - We need O(1) space
   - We can modify the input array

5. **Time Complexity:** O(N) - we traverse the array twice: once to mark, once to find missing numbers.
6. **Space Complexity:** O(1) - only the result list uses extra space, which doesn't count per the problem statement.

This problem shows how to use **array indexing patterns** to solve problems that would normally require hash tables, achieving O(1) space complexity.

**Code:**

```python
class Solution:
    def findDisappearedNumbers(self, nums: List[int]) -> List[int]:
        res = []
        for n in nums:
            i = abs(n) - 1
            nums[i] = -1 * abs(nums[i])
        for i, n in enumerate(nums):
            if n > 0:
                res.append(i+1)
        return res
```

---

## Problem: 1089. Duplicate Zeros

Given a fixed-length integer array `arr`, duplicate each occurrence of zero, shifting the remaining elements to the right.

Note that elements beyond the length of the original array are not written. Do the above modifications to the input array in place and do not return anything.

### Example 1

```
Input: arr = [1,0,2,3,0,4,5,0]
Output: [1,0,0,2,3,0,0,4]
Explanation: After calling your function, the input array is modified to: [1,0,0,2,3,0,0,4]
```

### Example 2

```
Input: arr = [1,2,3]
Output: [1,2,3]
Explanation: After calling your function, the input array is modified to: [1,2,3]
```

### Constraints

- `1 <= arr.length <= 10^4`
- `0 <= arr[i] <= 9`

### Solution

**Logic:**

This problem requires careful two-pointer manipulation from the end to avoid overwriting elements that haven't been processed yet. The key insight is to work backwards, calculating where each element should end up.

1. **Calculate Total Zeros:** First, count the total number of zeros in the array. This tells us how much the array will "expand" (though it's fixed-length, so some elements will be lost).

2. **Two-Pointer Setup from the End:**

   - `i`: reads from the original array (starts at `n-1`, the last element)
   - `j`: writes to the "expanded" array (starts at `n + zeros - 1`, accounting for duplicates)

3. **The Backward Strategy:** Work from right to left:

   - If `arr[i] == 0`:
     - Write the zero at position `j` (if `j < n`)
     - Decrement `j` and write another zero (duplicate)
     - Decrement `i` to move to the next element
   - If `arr[i] != 0`:
     - Write the element at position `j` (if `j < n`)
     - Decrement both `i` and `j`

4. **Why This Works:**

   - Working backwards ensures we don't overwrite elements we haven't processed yet
   - When we encounter a zero, we write it twice (the duplicate)
   - When we encounter a non-zero, we write it once
   - The `j < n` check ensures we don't write beyond the array bounds

5. **Key Insight:** This demonstrates the importance of **direction** in two-pointer problems. Sometimes working backwards is necessary to avoid overwriting unprocessed data. This is similar to the "merge from end" technique in merge operations.

6. **Time Complexity:** O(N) - we traverse the array once to count zeros, then once more to perform the duplication.
7. **Space Complexity:** O(1) - only a few variables are used.

This problem shows how **two pointers can work backwards** to handle in-place modifications that would be difficult to do forward.

**Code:**

```python
class Solution:
    def duplicateZeros(self, arr: List[int]) -> None:
        n = len(arr)
        zeros = arr.count(0)
        i = n - 1
        j = n + zeros - 1
        while i >= 0 and j >= 0:
            if j < n:
                arr[j] = arr[i]
            if arr[i] == 0:
                j -= 1
                if j < n:
                    arr[j] = 0
            i -= 1
            j -= 1
```
