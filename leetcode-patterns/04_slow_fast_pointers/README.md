# Slow & Fast Pointers Pattern

## Phase 1: Detecting Cycles & Relative Positions

This phase (5 problems) establishes the foundational Floyd’s Tortoise and Hare ideas. We first learn to detect cycles, then reuse the same relative-speed trick to find specific nodes (cycle start, list middle, nth from end) and even detect numerical cycles.

- **Problems in this Phase:**
  1. `141. Linked List Cycle`
     - **Reasoning:** Introduces the core technique: move one pointer twice as fast as the other, and if a cycle exists they eventually meet. This is the canonical Floyd’s algorithm and sets up all subsequent problems.
  2. `142. Linked List Cycle II`
     - **Reasoning:** Extends 141 by proving that after the first meeting we can restart one pointer at the head and advance both at the same speed to find the cycle’s entry node. Highlights how math behind cycle lengths lets us locate specific nodes.
  3. `876. Middle of the Linked List`
     - **Reasoning:** Demonstrates that relative speeds also help locate the midpoint: when the fast pointer hits the end, the slow pointer sits in the middle. Builds intuition that slow/fast pointers are not only for cycles.
  4. `19. Remove Nth Node from End of List`
     - **Reasoning:** Shows another perspective: instead of moving at different speeds simultaneously, we can offset one pointer by `n` steps and then move both equally to land on a target position from the end. Reinforces relative-motion thinking.
  5. `202. Happy Number`
     - **Reasoning:** Applies Floyd’s algorithm beyond linked lists by treating repeated digit-square transformations as a “next” pointer. Proves the pattern works for any deterministic sequence, not just node-based structures.

## Phase 2: Arrays as Linked Structures & In-Place Filtering

This phase (5 problems) marries the slow/fast technique with in-place data manipulation. We treat arrays as implicit linked lists (cycle detection) and reuse the two-pointer mindset for overwriting or skipping data without extra memory.

- **Problems in this Phase:**
  1. `287. Find the Duplicate Number`
     - **Reasoning:** The array version of Floyd’s cycle detection: interpret `nums[i]` as a “next pointer.” The duplicate value forms a cycle entry, so meeting + restart logic from 142 finds it without modifying the array.
  2. `26. Remove Duplicates from Sorted Array`
     - **Reasoning:** Introduces the slow/fast write/read pointers pattern for arrays. The fast pointer scans, the slow pointer overwrites only when a new unique value appears, providing the first in-place filtering example.
  3. `83. Remove Duplicates from Sorted List`
     - **Reasoning:** Linked-list analogue of Problem 26. Reinforces that the same “writer” pointer can live on nodes too by rewiring `next` pointers once a new value is discovered.
  4. `27. Remove Element`
     - **Reasoning:** Generalizes the filtering idea: instead of deduplicating, we filter by condition (`!= val`). Highlights that the same two-pointer approach works for any predicate, not just equality checks between neighbors.
  5. `61. Rotate List`
     - **Reasoning:** Synthesizes multiple tricks: measure list length, normalize `k`, create a cycle by linking tail to head, then walk a slow pointer to break it at the right spot. Demonstrates combining counting, modular arithmetic, and pointer rewiring.

## Phase 3: Advanced Pointer Coordination & Index Tricks

This phase (4 problems) pushes the pattern into multi-structure coordination, half-list reversals, and clever index marking. We now combine earlier ideas to solve harder problems in O(1) space.

- **Problems in this Phase:**
  1. `160. Intersection of Two Linked Lists`
     - **Reasoning:** Uses pointer “switching” between two lists to equalize path lengths. Both pointers traverse `lenA + lenB`, guaranteeing they meet at the intersection or `None`. Demonstrates coordinating pointers across disjoint structures.
  2. `234. Palindrome Linked List`
     - **Reasoning:** Combines several previous skills: find the middle (Phase 1), reverse the second half in-place, then walk two pointers to compare halves. Showcases pointer reversal as a tool for bidirectional comparisons.
  3. `448. Find All Numbers Disappeared in an Array`
     - **Reasoning:** Applies the “array as data structure” mindset by using indices as hash buckets. We mark visited values by flipping signs and then scan for untouched indices, reinforcing how pointer ideas extend to index manipulation.
  4. `1089. Duplicate Zeros`
     - **Reasoning:** Introduces backward two-pointer processing. Reading from the end while writing to a shifted index avoids overwriting untouched elements, a critical technique whenever forward traversal would corrupt upcoming data.

Together, these three phases take you from pure cycle detection to sophisticated pointer orchestration across lists, arrays, and numeric sequences while preserving O(1) extra space.
