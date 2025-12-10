# Pattern: Linked List In-place Reversal - Phase 1

## Problem 206. Reverse Linked List

Given the `head` of a singly linked list, reverse the list, and return the reversed list.

### Example 1

```
Input: head = [1,2,3,4,5]
Output: [5,4,3,2,1]
```

### Example 2

```
Input: head = [1,2]
Output: [2,1]
```

### Example 3

```
Input: head = []
Output: []
```

### Constraints

- The number of nodes in the list is the range `[0, 5000]`.
- `-5000 <= Node.val <= 5000`

Follow up: A linked list can be reversed either iteratively or recursively. Could you implement both?

### Solution

**Logic:**

This is the foundational problem for linked list reversal. The key insight is to use three pointers to reverse the links in-place: `prevN` (previous node), `currN` (current node), and `nextN` (next node to process).

1. **Three-Pointer Setup:**

   - `prevN = None` (will become the new head after reversal)
   - `currN = head` (current node being processed)
   - `nextN` (temporarily stores the next node)

2. **The Reversal Process:** For each node:

   - Save the next node: `nextN = currN.next`
   - Reverse the link: `currN.next = prevN`
   - Move pointers forward: `prevN = currN`, `currN = nextN`
   - Continue until `currN` is `None`

3. **Why This Works:** By reversing each link and moving forward, we build the reversed list from the end. When `currN` becomes `None`, `prevN` points to the last node, which is now the new head.

4. **Time Complexity:** O(N) - we traverse the list once.
5. **Space Complexity:** O(1) - only three pointers are used.

This problem establishes the fundamental **three-pointer reversal technique** that will be extended in subsequent problems.

**Code:**

```python
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        prevN = None
        currN = head
        while currN:
            nextN = currN.next
            currN.next = prevN
            prevN = currN
            currN = nextN
        return prevN
```

## Problem: 92. Reversed Linked List II

Given the `head` of a singly linked list and two integers `left` and `right` where `left <= right`, reverse the nodes of the list from position `left` to position `right`, and return the reversed list.

### Example 1

```
Input: head = [1,2,3,4,5], left = 2, right = 4
Output: [1,4,3,2,5]
```

### Example 2

```
Input: head = [5], left = 1, right = 1
Output: [5]
```

### Constraints

- The number of nodes in the list is n.
- `1 <= n <= 500`
- `-500 <= Node.val <= 500`
- `1 <= left <= right <= n`

**Follow up:** Could you do it in one pass?

### Solution

**Logic:**

This problem extends Problem 206 by reversing only a portion of the list. The key is to find the boundaries, reverse the sublist using the same three-pointer technique, and reconnect it properly.

1. **Finding the Subsection:** Traverse until reaching position `left`, keeping track of the node before the reversal section (`prevN`).

2. **Reversing the Subsection:** Once at position `left`:

   - Use the same three-pointer reversal technique from Problem 206
   - Reverse nodes from `left` to `right`
   - Store `leftN` (the first node of the reversed section) to reconnect later

3. **Reconnecting:** After reversal:

   - If `prevN` exists, connect it to the new head of the reversed section (`prevL`)
   - Connect `leftN.next` to the node after the reversed section (`currN`)

4. **Edge Case:** If `left == 1`, the entire list is reversed, so return `prevL` instead of `head`.

5. **Time Complexity:** O(N) - we traverse the list once.
6. **Space Complexity:** O(1) - only a few pointers are used.

This problem demonstrates how to **apply the reversal technique to a subsection** of a linked list.

**Code:**

```python
class Solution:
    def reverseBetween(self, head: Optional[ListNode], left: int, right: int) -> Optional[ListNode]:
        currN, prevN = head, None
        count = 0
        while currN:
            count += 1
            if count == left:
                prevL = None
                leftN = currN
                while count <= right:
                    count += 1
                    tempN = currN.next
                    currN.next = prevL
                    prevL = currN
                    currN = tempN
                if prevN:
                    prevN.next = prevL
                leftN.next = currN
            prevN = currN
            currN = currN.next if currN else None
        return prevL if left == 1 else head
```

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

## Problem: 24. Swap Nodes in Pairs

Given a linked list, swap every two adjacent nodes and return its `head`. You must solve the problem without modifying the values in the list's nodes (i.e., only nodes themselves may be changed.)

### Example 1

```
Input: head = [1,2,3,4]
Output: [2,1,4,3]
```

### Example 2

```
Input: head = []
Output: []
```

### Example 3

```
Input: head = [1]
Output: [1]
```

### Example 4

```
Input: head = [1,2,3]
Output: [2,1,3]
```

### Constraints

- The number of nodes in the list is in the range `[0, 100]`.
- `0 <= Node.val <= 100`

### Solution

**Logic:**

This problem is a special case of Problem 25 with `k = 2`. We swap adjacent pairs by manipulating pointers to reverse pairs of nodes.

1. **Dummy Head Setup:** Use a dummy head to simplify edge cases and handle the case where we need to update the head.

2. **Pair Swapping:** For each pair:

   - Store `new_next = curr.next.next` (the node after the pair)
   - Connect `prev.next` to `curr.next` (the second node becomes first)
   - Update `prev` to `curr` (the first node, which will be second)
   - Connect `curr.next.next` to `curr` (complete the swap)
   - Connect `curr.next` to `new_next` (link to the next pair)
   - Move `curr` to `new_next` (process next pair)

3. **Why This Works:** By carefully reordering the `next` pointers, we swap pairs without modifying values. The dummy head ensures we can always update `prev.next`.

4. **Time Complexity:** O(N) - we traverse the list once.
5. **Space Complexity:** O(1) - only a few pointers are used.

This problem demonstrates **pairwise swapping** as a simplified case of group reversal.

**Code:**

```python
class Solution:
    def swapPairs(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if not head:
            return head
        curr = head
        new_head = ListNode (0, head)
        prev = new_head
        while curr and curr.next:
            new_next = curr.next.next
            prev.next = curr.next
            prev = curr
            curr.next.next = curr
            curr.next = new_next
            curr = curr.next

        return new_head.next
```

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

## Problem: 445. Add Two Numbers II

You are given two non-empty linked lists representing two non-negative integers. The most significant digit comes first and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

### Example 1

```
Input: l1 = [7,2,4,3], l2 = [5,6,4]
Output: [7,8,0,7]
```

### Example 2

```
Input: l1 = [2,4,3], l2 = [5,6,4]
Output: [8,0,7]
```

### Example 3

```
Input: l1 = [0], l2 = [0]
Output: [0]
```

### Constraints

- The number of nodes in each linked list is in the range `[1, 100]`.
- `0 <= Node.val <= 9`
- It is guaranteed that the list represents a number that does not have leading zeros.

### Solution

**Logic:**

**Code:**

```python
class Solution:
    def addTwoNumbers(self, l1: Optional[ListNode], l2: Optional[ListNode]) -> Optional[ListNode]:

        list1 = self.reverseList(l1)
        list2 = self.reverseList(l2)
        carry = 0
        res = ListNode()
        head = res

        while list1 or list2 or carry:
            val1 = list1.val if list1 else 0
            val2 = list2.val if list2 else 0
            sum_val = val1 + val2 + carry
            new_val = sum_val % 10
            carry = sum_val // 10
            res.next = ListNode(new_val, None)
            res = res.next
            list1 = list1.next if list1 else None
            list2 = list2.next if list2 else None
        ListNode(carry, None)

        return self.reverseList(head.next)


    def reverseList(self, head):
        prev = None
        while head:
            temp = head.next
            head.next = prev
            prev = head
            head = temp
        return prev
```

## Problem: 2130. Maximum Twin Sum of a Linked List

In a linked list of size `n`, where `n` is even, the `ith` node (0-indexed) of the linked list is known as the twin of the `(n-1-i)th` node, if `0 <= i <= (n / 2) - 1`.

For example, if `n = 4`, then node `0` is the twin of node `3`, and node `1` is the twin of node `2`. These are the only nodes with twins for `n = 4`.
The twin sum is defined as the sum of a node and its twin.

Given the `head` of a linked list with even length, return the maximum twin sum of the linked list.

### Example 1

```
Input: head = [5,4,2,1]
Output: 6
Explanation:
Nodes 0 and 1 are the twins of nodes 3 and 2, respectively. All have twin sum = 6.
There are no other nodes with twins in the linked list.
Thus, the maximum twin sum of the linked list is 6.
```

### Example 2

```
Input: head = [4,2,2,3]
Output: 7
Explanation:
The nodes with twins present in this linked list are:
- Node 0 is the twin of node 3 having a twin sum of 4 + 3 = 7.
- Node 1 is the twin of node 2 having a twin sum of 2 + 2 = 4.
Thus, the maximum twin sum of the linked list is max(7, 4) = 7.
```

### Example 3

```
Input: head = [1,100000]
Output: 100001
Explanation:
There is only one node with a twin in the linked list having twin sum of 1 + 100000 = 100001.
```

### Constraints

- The number of nodes in the list is an even integer in the range `[2, 10^5]`.
- `1 <= Node.val <= 105`

### Solution

**Logic:**

This problem uses the same approach as Problem 143: find the middle, reverse the second half, then compare corresponding nodes. However, instead of reordering, we calculate the sum of twin pairs.

1. **Find the Middle:** Use slow/fast pointers. Since the list has even length, `slow.next` is the start of the second half.

2. **Reverse the Second Half:** Apply the reversal technique from Problem 206 to reverse the second half.

3. **Calculate Twin Sums:** Traverse both halves simultaneously:

   - For each pair of corresponding nodes, calculate `left.val + right.val`
   - Track the maximum sum

4. **Why This Works:** After reversing the second half, the first node of the reversed half pairs with the first node of the first half, the second with the second, etc., which are exactly the twin pairs.

5. **Time Complexity:** O(N) - we traverse the list multiple times but each node is visited a constant number of times.
6. **Space Complexity:** O(1) - only pointers are used.

This problem demonstrates using **reversal to pair nodes from opposite ends** for computation.

**Code:**

```python
class Solution:
    def pairSum(self, head: Optional[ListNode]) -> int:
        slow = head
        fast = head.next
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next

        right = slow.next
        slow.next = None
        prev = None
        while right:
            temp = right.next
            right.next= prev
            prev = right
            right = temp
        right = prev

        left = head
        res = 0
        while right:
            res = max(res, left.val + right.val)
            left = left.next
            right = right.next
        return res
```

## Problem: 2487. Remove Nodes From Linked List

You are given the `head` of a linked list.

Remove every node which has a node with a greater value anywhere to the right side of it.

Return the `head` of the modified linked list.

### Example 1

```
Input: head = [5,2,13,3,8]
Output: [13,8]
Explanation: The nodes that should be removed are 5, 2 and 3.
- Node 13 is to the right of node 5.
- Node 13 is to the right of node 2.
- Node 8 is to the right of node 3.
```

### Example 2

```
Input: head = [1,1,1,1]
Output: [1,1,1,1]
Explanation: Every node has value 1, so no nodes are removed.
```

### Constraints

- The number of the nodes in the given list is in the range `[1, 10^5]`.
- `1 <= Node.val <= 10^5`

### Solution

**Logic:**

This problem uses reversal as a technique to process the list from right to left. By reversing the list, we can easily identify and remove nodes that have a greater value to their right.

1. **Reverse the List:** Apply the reversal technique from Problem 206 to reverse the entire list.

2. **Remove Nodes:** Traverse the reversed list from left to right (which is right to left in the original):

   - Track the maximum value seen so far
   - If a node's value is less than the current maximum, remove it (it has a greater value to its right in the original)
   - Otherwise, update the maximum and keep the node

3. **Reverse Again:** Reverse the list back to restore the original order.

4. **Why This Works:** After reversal, processing from left to right is equivalent to processing the original list from right to left. This makes it easy to identify nodes with greater values to their right.

5. **Time Complexity:** O(N) - we traverse the list multiple times.
6. **Space Complexity:** O(1) - only pointers are used.

This problem demonstrates using **reversal to change traversal direction** for easier problem solving.

**Code:**

```python
class Solution:
    def removeNodes(self, head: Optional[ListNode]) -> Optional[ListNode]:
        prev = slow = self.reverseList(head)
        fast = slow.next
        curr_max = slow.val
        while fast:
            curr_max = max(curr_max, fast.val)
            if fast.val < curr_max:
                slow.next = fast.next
            else:
                slow = slow.next
            fast = fast.next
        return self.reverseList(prev)

    def reverseList(self, curr):
        prev = None
        while curr:
            temp = curr.next
            curr.next = prev
            prev = curr
            curr = temp
        return prev
```

## 203. Remove Linked List Elements

Given the `head` of a linked list and an integer `val`, remove all the nodes of the linked list that has `Node.val == val`, and return the new `head`.

### Example 1

```
Input: head = [1,2,6,3,4,5,6], val = 6
Output: [1,2,3,4,5]
```

### Example 2

```
Input: head = [], val = 1
Output: []
```

### Example 3

```
Input: head = [7,7,7,7], val = 7
Output: []
```

### Constraints

- The number of nodes in the list is in the range `[0, 10^4]`.
- `1 <= Node.val <= 50`
- `0 <= val <= 50`

### Solution

**Logic:**

This problem uses a two-pointer technique with a dummy head to filter out nodes with a specific value. The slow pointer tracks the last valid node, while the fast pointer scans for nodes to keep.

1. **Dummy Head Setup:** Create a dummy head to simplify edge cases, especially when the head needs to be removed.

2. **Two-Pointer Filtering:**

   - `slow`: points to the last node we've kept
   - `fast`: scans through the list
   - If `fast.val == val`, skip it by connecting `slow.next` to `fast.next`
   - Otherwise, move `slow` forward

3. **Why This Works:** By maintaining `slow` as the last valid node, we can easily skip unwanted nodes by updating `slow.next`. The dummy head ensures we always have a valid `slow` pointer.

4. **Time Complexity:** O(N) - we traverse the list once.
5. **Space Complexity:** O(1) - only a few pointers are used.

This problem demonstrates **two-pointer filtering** for removing specific values from a linked list.

**Code:**

```python
class Solution:
    def removeElements(self, head: Optional[ListNode], val: int) -> Optional[ListNode]:
        if not head:
            return head
        new_head = ListNode(0, head)
        slow = new_head
        fast = slow.next
        while fast:
            if fast.val == val:
                slow.next = fast.next
            else:
                slow = slow.next
            fast = fast.next
        return new_head.next
```

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

## Problem: 1721. Swapping Nodes in a Linked List

You are given the `head` of a linked list, and an integer `k`.

Return the `head` of the linked list after swapping the values of the `kth` node from the beginning and the `kth` node from the end (the list is 1-indexed).

### Example 1

```
Input: head = [1,2,3,4,5], k = 2
Output: [1,4,3,2,5]
```

### Example 2

```
Input: head = [7,9,6,6,7,8,3,0,9,5], k = 5
Output: [7,9,6,6,8,7,3,0,9,5]
```

### Constraints

- The number of nodes in the list is `n`.
- `1 <= k <= n <= 10^5`
- `0 <= Node.val <= 100`

### Solution

**Logic:**

This problem uses two pointers with a fixed offset to find the kth node from the end (similar to Problem 19), then swaps values of the kth node from the beginning and the kth node from the end.

1. **Find kth from Beginning:** Traverse `k-1` steps from head to reach the kth node (`left`).

2. **Find kth from End:** Use two pointers with offset:

   - Start `right` at head and `curr` at `left`
   - Move both forward until `curr` reaches the end
   - `right` will be at the kth node from the end

3. **Swap Values:** Simply swap the values of `left` and `right` nodes.

4. **Why This Works:** By maintaining a `k`-node gap between `right` and `curr`, when `curr` reaches the end, `right` is exactly `k` positions from the end.

5. **Time Complexity:** O(N) - we traverse the list once.
6. **Space Complexity:** O(1) - only pointers are used.

This problem demonstrates **finding nodes by position** and swapping their values.

**Code:**

```python
class Solution:
    def swapNodes(self, head: Optional[ListNode], k: int) -> Optional[ListNode]:
        left = head
        for i in range(k - 1):
            left = left.next
        right = head
        curr = left
        while curr.next:
            curr = curr.next
            right = right.next
        left.val, right.val = right.val, left.val
        return head
```

## 138. Copy List with Random Pointer

A linked list of length `n` is given such that each node contains an additional random pointer, which could point to any node in the list, or `null`.

Construct a deep copy of the list. The deep copy should consist of exactly `n` brand new nodes, where each new node has its value set to the value of its corresponding original node. Both the `next` and `random` pointer of the new nodes should point to new nodes in the copied list such that the pointers in the original list and copied list represent the same list state. None of the pointers in the new list should point to nodes in the original list.

For example, if there are two nodes `X` and `Y` in the original list, where `X.random --> Y`, then for the corresponding two nodes `x` and `y` in the copied list, `x.random --> y`.

Return the head of the copied linked list.

The linked list is represented in the input/output as a list of n nodes. Each node is represented as a pair of `[val, random_index]` where:

`val`: an integer representing `Node.val`
`random_index`: the index of the node (range from `0` to `n-1`) that the random pointer points to, or `null` if it does not point to any node.
Your code will only be given the `head` of the original linked list.

### Example 1

```
Input: head = [[7,null],[13,0],[11,4],[10,2],[1,0]]
Output: [[7,null],[13,0],[11,4],[10,2],[1,0]]
```

### Example 2

```
Input: head = [[1,1],[2,1]]
Output: [[1,1],[2,1]]
```

### Example 3

```
Input: head = [[3,null],[3,0],[3,null]]
Output: [[3,null],[3,0],[3,null]]
```

### Constraints

- `0 <= n <= 1000`
- `-10^4 <= Node.val <= 10^4`
- `Node.random` is `null` or is pointing to some node in the linked list.

### Solution

**Logic:**

This problem requires creating a deep copy of a linked list with random pointers. The key is to use a hash map to maintain the mapping between original and copied nodes.

1. **First Pass - Create Nodes:** Traverse the original list and create a copy of each node, storing the mapping `{original_node: copy_node}` in a hash map. Include `None` in the map to handle null pointers.

2. **Second Pass - Set Pointers:** Traverse the original list again:

   - For each original node, get its copy from the map
   - Set `copy.next = copy_map[original.next]` (using the map to get the copied next node)
   - Set `copy.random = copy_map[original.random]` (using the map to get the copied random node)

3. **Why This Works:** The hash map ensures that when we set pointers in the second pass, we can always find the corresponding copied node, even if it was created earlier.

4. **Time Complexity:** O(N) - we traverse the list twice.
5. **Space Complexity:** O(N) - the hash map stores all nodes.

This problem demonstrates **deep copying with pointer relationships** using a hash map for node mapping.

**Code:**

```python
class Solution:
    def copyRandomList(self, head: 'Optional[Node]') -> 'Optional[Node]':
        copy_map = {None: None}
        curr = head

        while curr:
            copy = Node(curr.val)
            copy_map[curr] = copy
            curr = curr.next

        curr = head
        while curr:
            copy = copy_map[curr]
            copy.next = copy_map[curr.next]
            copy.random = copy_map[curr.random]
            curr = curr.next

        return copy_map[head]
```

## 2. Add Two Numbers

You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

### Example 1

```
Input: l1 = [2,4,3], l2 = [5,6,4]
Output: [7,0,8]
Explanation: 342 + 465 = 807.
```

### Example 2

```
Input: l1 = [0], l2 = [0]
Output: [0]
```

### Example 3

```
Input: l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
Output: [8,9,9,9,0,0,0,1]
```

### Constraints

- The number of nodes in each linked list is in the range `[1, 100]`.
- `0 <= Node.val <= 9`
- It is guaranteed that the list represents a number that does not have leading zeros.

### Solution

**Logic:**

This is the foundational problem for adding numbers represented as linked lists. Since digits are stored in reverse order (least significant first), we can add them naturally from left to right.

1. **Dummy Head Setup:** Create a dummy head to simplify result construction.

2. **Addition Process:** Traverse both lists simultaneously:

   - Extract values from both lists (0 if a list is exhausted)
   - Calculate `sum = val1 + val2 + carry`
   - Create a new node with value `sum % 10`
   - Update `carry = sum // 10`
   - Continue until both lists are exhausted and carry is 0

3. **Why This Works:** Since digits are in reverse order, we process them from least to most significant, which matches how we perform addition. The carry propagates naturally.

4. **Time Complexity:** O(max(M, N)) where M and N are the lengths of the lists.
5. **Space Complexity:** O(max(M, N)) for the result list.

This problem establishes the **standard linked list addition algorithm** that Problem 445 builds upon with reversal.

**Code:**

```python
class Solution:
    def addTwoNumbers(self, l1: Optional[ListNode], l2: Optional[ListNode]) -> Optional[ListNode]:
        res = ListNode()
        res_head = res
        carry = 0
        while l1 or l2 or carry:
            res.next = ListNode()
            res = res.next
            val = 0
            val += l1.val if l1 else 0
            val += l2.val if l2 else 0
            val += carry
            res.val = val % 10
            carry = val // 10
            l1 = l1.next if l1 else None
            l2 = l2.next if l2 else None
        return res_head.next
```
