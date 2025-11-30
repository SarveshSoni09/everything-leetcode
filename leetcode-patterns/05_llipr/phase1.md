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
