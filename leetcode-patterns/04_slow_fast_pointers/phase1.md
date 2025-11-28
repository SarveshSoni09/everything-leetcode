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

This is the foundational problem for the slow and fast pointer pattern (also known as Floyd's Cycle Detection Algorithm). The key insight is that if there's a cycle, a fast pointer moving at twice the speed of a slow pointer will eventually catch up to the slow pointer.

1. **Initial Setup:** Both `slow` and `fast` pointers start at the `head` of the linked list.

2. **The Movement Strategy:**

   - `slow` moves one step at a time: `slow = slow.next`
   - `fast` moves two steps at a time: `fast = fast.next.next`
   - This creates a relative speed difference of 1 step per iteration

3. **Cycle Detection:** If there's a cycle, the fast pointer will eventually "lap" the slow pointer and they will meet at some node. If there's no cycle, the fast pointer will reach the end (`None`) first.

4. **Why This Works:** In a cycle, the fast pointer gains one position on the slow pointer per iteration. Since the cycle has finite length, the fast pointer will eventually catch up. The mathematical proof shows that they will meet within one cycle length.

5. **Time Complexity:** O(N) - in the worst case, we traverse the list once.
6. **Space Complexity:** O(1) - only two pointers are used, regardless of list size.

This problem establishes the fundamental slow/fast pointer technique: **use two pointers moving at different speeds to detect cycles**.

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

---

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

This problem builds directly on Problem 141. After detecting a cycle, we need to find where the cycle starts. The key mathematical insight is that after the slow and fast pointers meet, if we start one pointer from the head and another from the meeting point, moving both at the same speed, they will meet at the cycle start.

1. **Cycle Detection Phase:** First, use the slow/fast pointer technique from Problem 141 to detect if a cycle exists. If `slow == fast` at some point, we have a cycle.

2. **Finding Cycle Start:** After detecting the cycle:

   - Keep `fast` at the meeting point
   - Start a new pointer `start` from the `head`
   - Move both `start` and `fast` one step at a time
   - They will meet at the cycle start node

3. **Why This Works:** The mathematical proof shows that the distance from the head to the cycle start equals the distance from the meeting point to the cycle start (when going around the cycle). This is why moving both pointers at the same speed from these two positions causes them to meet at the cycle start.

4. **Key Insight:** This demonstrates that slow/fast pointers can do more than just detect cycles - they can also locate specific nodes in cycles through careful pointer manipulation.

5. **Time Complexity:** O(N) - we traverse the list at most twice.
6. **Space Complexity:** O(1) - only three pointers are used.

This problem extends Problem 141 by showing how to **locate the cycle start** after detection.

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

---

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

This problem demonstrates a simple but powerful application of slow/fast pointers: finding the middle of a linked list in a single pass without knowing the length beforehand.

1. **Initial Setup:** Both `slow` and `fast` pointers start at the `head`.

2. **The Movement Strategy:**

   - `slow` moves one step: `slow = slow.next`
   - `fast` moves two steps: `fast = fast.next.next`
   - Continue until `fast` reaches the end

3. **Why This Works:** When the fast pointer reaches the end (or `None`), the slow pointer will be at the middle. This is because:

   - If the list has `n` nodes, the fast pointer travels `n` steps
   - The slow pointer travels `n/2` steps, which is exactly the middle
   - For even-length lists, `fast` will be at `None` and `slow` will be at the second middle node

4. **Key Insight:** The slow/fast pointer technique naturally finds the middle because the fast pointer travels twice as far, so when it finishes, the slow pointer is halfway.

5. **Time Complexity:** O(N) - we traverse the list once.
6. **Space Complexity:** O(1) - only two pointers are used.

This problem shows that slow/fast pointers are useful for **finding positions based on relative distances**, not just cycle detection.

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

---

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

This problem demonstrates using two pointers with a **fixed offset** to find the nth node from the end. The key insight is that if we maintain `n` nodes between two pointers, when the fast pointer reaches the end, the slow pointer will be at the node before the one we need to remove.

1. **Initial Setup:** Both `slow` and `fast` start at `head`. First, advance `fast` by `n` positions to create the offset.

2. **Edge Case Handling:** If `fast` reaches `None` after advancing `n` steps, it means we need to remove the head node. Return `head.next` in this case.

3. **The Movement Strategy:**

   - Move both `slow` and `fast` one step at a time
   - Continue until `fast.next` is `None` (fast is at the last node)
   - At this point, `slow` is at the node **before** the one to remove

4. **Removal:** Set `slow.next = slow.next.next` to skip the nth node from the end.

5. **Why This Works:** By maintaining an `n`-node gap between the pointers, when the fast pointer reaches the end, the slow pointer is exactly `n` positions behind, which is the node before the one we need to remove.

6. **Key Insight:** This demonstrates that two pointers don't always need to move at different speeds - they can move at the same speed but start with an offset to find positions relative to the end.

7. **Time Complexity:** O(N) - we traverse the list once.
8. **Space Complexity:** O(1) - only two pointers are used.

This problem shows how to use **two pointers with a fixed offset** to find positions from the end of a list.

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

---

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

This problem applies the cycle detection pattern from Problem 141 to a number sequence. The transformation of a number (sum of squares of digits) creates a sequence that either reaches 1 (happy) or enters a cycle (unhappy). We can use slow/fast pointers to detect this cycle.

1. **The Transformation:** Create a helper function that computes the sum of squares of digits. This transformation is applied repeatedly to create a sequence.

2. **Cycle Detection:** Use slow/fast pointers:

   - `slow` applies the transformation once per step
   - `fast` applies the transformation twice per step
   - If we reach 1, the number is happy
   - If `slow == fast` (and it's not 1), we've detected a cycle, so the number is unhappy

3. **Why This Works:** The sequence of transformed numbers will either:

   - Reach 1 and stay at 1 (happy number)
   - Enter a cycle (unhappy number)

   The slow/fast pointer technique detects cycles just like in Problem 141.

4. **Key Insight:** This demonstrates that the slow/fast pointer pattern applies to **any sequence** where we can define a "next" operation, not just linked lists. The pattern works for detecting cycles in mathematical sequences.

5. **Time Complexity:** O(log N) - the number of digits in `n` is logarithmic, and we detect cycles quickly.
6. **Space Complexity:** O(1) - only a few variables are used, no extra space for storing visited numbers.

This problem shows that slow/fast pointers can detect cycles in **abstract sequences**, not just linked list structures.

**Code:**

```python
class Solution:
    def isHappy(self, n: int) -> bool:
        def get_next(num):
            total = 0
            while num > 0:
                num, digit = divmod(num, 10)
                total += digit ** 2
            return total

        slow = n
        fast = get_next(n)

        while fast != 1 and slow != fast:
            slow = get_next(slow)
            fast = get_next(get_next(fast))

        return fast == 1
```
