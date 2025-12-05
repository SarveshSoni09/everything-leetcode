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
