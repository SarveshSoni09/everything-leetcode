# Pattern: Linked List In-place Reversal - Phase 3

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

This problem uses reversal to convert the problem to the standard "Add Two Numbers" format (Problem 2). Since digits are stored most-significant first, we reverse both lists, add them (as in Problem 2), then reverse the result.

1. **Reverse Both Lists:** Apply the reversal technique from Problem 206 to both `l1` and `l2` to make them least-significant first.

2. **Add the Numbers:** Use the standard addition algorithm from Problem 2:

   - Process digits from both lists along with carry
   - Create new nodes for the sum
   - Handle remaining digits and final carry

3. **Reverse the Result:** Reverse the result list to restore most-significant-first format.

4. **Why This Works:** By reversing, we convert the problem to the familiar format where we can add from right to left, then reverse back to the required format.

5. **Time Complexity:** O(max(M, N)) where M and N are the lengths of the lists.
6. **Space Complexity:** O(max(M, N)) for the result list.

This problem demonstrates using **reversal as a transformation technique** to simplify problem solving.

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

---

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

---

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

---

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

---

## Problem: 138. Copy List with Random Pointer

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

---

## Problem: 2. Add Two Numbers

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
