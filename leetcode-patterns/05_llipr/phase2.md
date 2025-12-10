# Pattern: Linked List In-place Reversal - Phase 2

## Problem: 25. Reverse Nodes in k-Group

Given the `head` of a linked list, reverse the nodes of the list `k` at a time, and return the modified list.

`k` is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of `k` then left-out nodes, in the end, should remain as it is.

You may not alter the values in the list's nodes, only nodes themselves may be changed.

### Example 1

```
Input: head = [1,2,3,4,5], k = 2
Output: [2,1,4,3,5]
```

### Example 2

```
Input: head = [1,2,3,4,5], k = 3
Output: [3,2,1,4,5]
```

### Constraints

- The number of nodes in the list is n.
- `1 <= k <= n <= 5000`
- `0 <= Node.val <= 1000`

**Follow-up:** Can you solve the problem in O(1) extra memory space?

### Solution

**Logic:**

This problem extends the reversal pattern to process the list in groups of `k`. The key is to repeatedly reverse groups of `k` nodes using the same three-pointer technique from Problem 206.

1. **Group Processing:** Use a dummy head to simplify edge cases. For each group:

   - Find the `kth` node from `group_prev` using `get_kth()`
   - If there aren't `k` nodes remaining, stop (leave them as-is)

2. **Reversing a Group:** Once we have `k` nodes:

   - Store `group_next = kth.next` (the node after this group)
   - Reverse the group using the three-pointer technique, with `prev = group_next` (points to the next group)
   - After reversal, `kth` becomes the new head of this group

3. **Reconnecting:**

   - Connect `group_prev.next` to `kth` (the new head of the reversed group)
   - Move `group_prev` to the last node of the current group (which was the first node before reversal)

4. **Why This Works:** By reversing each group and properly reconnecting, we process the list in chunks. The `group_prev` pointer maintains the connection between groups.

5. **Time Complexity:** O(N) - we visit each node once.
6. **Space Complexity:** O(1) - only a few pointers are used.

This problem demonstrates **repeated application of the reversal technique** to process the list in groups.

**Code:**

```python
class Solution:
    def reverseKGroup(self, head: Optional[ListNode], k: int) -> Optional[ListNode]:
        new_head = ListNode(0, head)
        group_prev = new_head

        while True:
            kth = self.get_kth(group_prev, k)
            if not kth:
                break
            group_next = kth.next
            prev, curr = kth.next, group_prev.next

            while curr != group_next:
                temp = curr.next
                curr.next = prev
                prev = curr
                curr = temp

            temp = group_prev.next
            group_prev.next = kth
            group_prev = temp

        return new_head.next

    def get_kth(self, curr, k):
        while curr and k > 0:
            curr = curr.next
            k -= 1
        return curr
```

---

## Problem: 143. Reorder List

You are given the `head` of a singly linked-list. The list can be represented as:

`L0 → L1 → … → Ln - 1 → Ln`
Reorder the list to be on the following form:

`L0 → Ln → L1 → Ln - 1 → L2 → Ln - 2 → …`
You may not modify the values in the list's nodes. Only nodes themselves may be changed.

### Example 1

```
Input: head = [1,2,3,4]
Output: [1,4,2,3]
```

### Example 2

```
Input: head = [1,2,3,4,5]
Output: [1,5,2,4,3]
```

### Constraints

- The number of nodes in the list is in the range `[1, 5 * 10^4]`.
- `1 <= Node.val <= 1000`

### Solution

**Logic:**

This problem combines multiple techniques: finding the middle (slow/fast pointers), reversing the second half (Problem 206), and interleaving two lists. The key is to split the list, reverse the second half, then merge them alternately.

1. **Find the Middle:** Use slow/fast pointers to find the middle node. Set `slow.next = None` to split the list into two halves.

2. **Reverse the Second Half:** Apply the three-pointer reversal technique from Problem 206 to reverse the second half (`l2`).

3. **Interleave the Lists:** Merge the two halves alternately:

   - Store `temp = l1.next` (save the next node in first half)
   - Connect `l1.next` to `l2` (link first half to second half)
   - Move `l2` forward
   - Connect `l1.next.next` to `temp` (link back to first half)
   - Move `l1` two steps forward

4. **Why This Works:** By reversing the second half and interleaving, we achieve the desired pattern: `L0 → Ln → L1 → Ln-1 → ...`

5. **Time Complexity:** O(N) - we traverse the list multiple times but each node is visited a constant number of times.
6. **Space Complexity:** O(1) - only pointers are used.

This problem demonstrates **combining reversal with list interleaving** to achieve complex reordering.

**Code:**

```python
class Solution:
    def reorderList(self, head: Optional[ListNode]) -> None:
        slow, fast = head, head.next
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
        l2 = slow.next
        slow.next = None

        prev = None
        while l2:
            temp = l2.next
            l2.next = prev
            prev = l2
            l2 = temp

        l1 = head
        l2 = prev
        while l2:
            temp = l1.next
            l1.next = l2
            l2 = l2.next
            l1.next.next = temp
            l1 = l1.next.next
        return head
```

---

## Problem: 82. Remove Duplicates from Sorted List II

Given the `head` of a sorted linked list, delete all nodes that have duplicate numbers, leaving only distinct numbers from the original list. Return the linked list sorted as well.

### Example 1

```
Input: head = [1,2,3,3,4,4,5]
Output: [1,2,5]
```

### Example 2

```
Input: head = [1,1,1,2,3]
Output: [2,3]
```

### Constraints

- The number of nodes in the list is in the range `[0, 300]`.
- `-100 <= Node.val <= 100`
- The list is guaranteed to be sorted in ascending order.

### Solution

**Logic:**

This problem extends the filtering pattern from Problem 203. Instead of removing a single value, we remove all nodes that have duplicates, keeping only nodes with unique values.

1. **Dummy Head Setup:** Use a dummy head to handle cases where the head needs to be removed.

2. **Two-Pointer Approach:**

   - `slow`: points to the last node we've kept
   - `fast`: scans through the list
   - When `fast.val == fast.next.val`, we've found duplicates
   - Skip all nodes with this value by moving `fast` until a different value is found
   - Connect `slow.next` to the new `fast` position (skipping all duplicates)

3. **Why This Works:** Since the list is sorted, duplicates are adjacent. When we find `fast.val == fast.next.val`, we can skip all consecutive nodes with this value, then connect `slow.next` to the next unique value.

4. **Time Complexity:** O(N) - we traverse the list once.
5. **Space Complexity:** O(1) - only pointers are used.

This problem demonstrates **removing all duplicates** (not just one occurrence) using the two-pointer filtering pattern.

**Code:**

```python
class Solution:
    def deleteDuplicates(self, head: Optional[ListNode]) -> Optional[ListNode]:
        new_head = ListNode(0, head)
        slow = new_head
        fast = new_head.next
        while fast and fast.next:
            curr_val = fast.val
            if fast.val == fast.next.val:
                while fast and fast.val == curr_val:
                    fast = fast.next
                slow.next = fast
            else:
                fast = fast.next
                slow = slow.next
        return new_head.next
```

---

## Problem: 86. Partition List

Given the `head` of a linked list and a value `x`, partition it such that all nodes less than `x` come before nodes greater than or equal to `x`.

You should preserve the original relative order of the nodes in each of the two partitions.

### Example 1

```
Input: head = [1,4,3,2,5,2], x = 3
Output: [1,2,2,4,3,5]
```

### Example 2

```
Input: head = [2,1], x = 2
Output: [1,2]
```

### Constraints

- The number of nodes in the list is in the range `[0, 200]`.
- `-100 <= Node.val <= 100`
- `-200 <= x <= 200`

### Solution

**Logic:**

This problem uses a two-list approach to partition nodes based on a value. We create two separate lists (small and big), then concatenate them.

1. **Two-List Setup:** Create dummy heads for both "small" (values < x) and "big" (values >= x) lists, along with tail pointers for efficient appending.

2. **Partitioning:** Traverse the original list:

   - For each node, detach it from the list (`curr.next = None`)
   - If `curr.val < x`, append to the small list
   - Otherwise, append to the big list
   - Update the respective tail pointer

3. **Concatenation:** After processing all nodes, connect the small list's tail to the big list's head.

4. **Why This Works:** By maintaining separate lists and their tails, we can efficiently partition while preserving relative order. The dummy heads simplify edge cases.

5. **Time Complexity:** O(N) - we traverse the list once.
6. **Space Complexity:** O(1) - only pointers are used, no new nodes created.

This problem demonstrates **partitioning using two separate lists** to maintain relative order.

**Code:**

```python
class Solution:
    def partition(self, head: Optional[ListNode], x: int) -> Optional[ListNode]:
        small_head, big_head = ListNode(0), ListNode(0)
        small_tail, big_tail = small_head, big_head
        curr = head
        while curr:
            temp = curr.next
            curr.next = None
            if curr.val < x:
                small_tail.next = curr
                small_tail = small_tail.next
            else:
                big_tail.next = curr
                big_tail = big_tail.next
            curr = temp
        small_tail.next = big_head.next
        return small_head.next
```

---

## Problem: 328. Odd Even Linked List

Given the `head` of a singly linked list, group all the nodes with odd indices together followed by the nodes with even indices, and return the reordered list.

The first node is considered odd, and the second node is even, and so on.

Note that the relative order inside both the even and odd groups should remain as it was in the input.

You must solve the problem in `O(1)` extra space complexity and `O(n)` time complexity.

### Example 1

```
Input: head = [1,2,3,4,5]
Output: [1,3,5,2,4]
```

### Example 2

```
Input: head = [2,1,3,5,6,4,7]
Output: [2,3,6,7,1,5,4]
```

### Constraints

- The number of nodes in the linked list is in the range `[0, 10^4]`.
- `-10^6 <= Node.val <= 10^6`

### Solution

**Logic:**

This problem uses the same two-list partitioning approach as Problem 86, but partitions based on index parity (odd vs even) rather than value comparison.

1. **Two-List Setup:** Create dummy heads for odd-indexed and even-indexed lists, with tail pointers for efficient appending.

2. **Alternating Partition:** Traverse the list, alternating between odd and even:

   - Use a boolean flag `odd` to track which list to append to
   - Detach each node and append to the appropriate list
   - Toggle the flag after each append

3. **Concatenation:** Connect the odd list's tail to the even list's head.

4. **Why This Works:** By maintaining separate lists and alternating appends, we preserve the relative order within each group while separating by index parity.

5. **Time Complexity:** O(N) - we traverse the list once.
6. **Space Complexity:** O(1) - only pointers are used.

This problem demonstrates **partitioning by index pattern** using the same two-list technique as Problem 86.

**Code:**

```python
class Solution:
    def oddEvenList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        odd_head, even_head = ListNode(0), ListNode(0)
        odd_tail, even_tail = odd_head, even_head
        curr = head
        odd = True
        while curr:
            temp = curr.next
            curr.next = None
            if odd:
                odd_tail.next = curr
                odd_tail = odd_tail.next
                odd = False
            else:
                even_tail.next = curr
                even_tail = even_tail.next
                odd = True
            curr = temp
        odd_tail.next = even_head.next
        return odd_head.next
```
