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

---

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

---

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

This problem is a special case of group reversal with `k = 2`. We swap adjacent pairs by manipulating pointers to reverse pairs of nodes.

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

---

## Problem: 203. Remove Linked List Elements

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
