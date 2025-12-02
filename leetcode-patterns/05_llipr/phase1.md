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
