# Linked List In-place Reversal Pattern

## Phase 1: Basic Reversal and Simple Operations

This phase (4 problems) establishes the foundational three-pointer reversal technique. We learn to reverse entire lists, reverse subsections, handle special cases like pairwise swapping, and apply basic filtering operations. These problems form the core building blocks for all advanced linked list manipulation.

- **Problems in this Phase:**
  1. `206. Reverse Linked List`
     - **Reasoning:** The foundational problem for linked list reversal. Introduces the core three-pointer technique: `prevN`, `currN`, and `nextN`. Demonstrates how to reverse links in-place by saving the next node, reversing the current link, and advancing pointers. This establishes the fundamental reversal pattern that all subsequent problems build upon.
  2. `92. Reverse Linked List II`
     - **Reasoning:** Extends Problem 206 by reversing only a portion of the list. Demonstrates how to find boundaries, apply the same three-pointer reversal technique to a subsection, and properly reconnect the reversed section with the rest of the list. Shows that the reversal technique can be applied selectively, not just to entire lists.
  3. `24. Swap Nodes in Pairs`
     - **Reasoning:** A special case of group reversal with `k = 2`. Demonstrates pairwise swapping by carefully reordering `next` pointers without modifying values. Introduces the dummy head pattern for simplifying edge cases. Shows how the reversal concept can be applied to smaller, fixed-size groups.
  4. `203. Remove Linked List Elements`
     - **Reasoning:** Introduces the two-pointer filtering pattern with a dummy head. While not strictly a reversal problem, it establishes the two-pointer technique (slow/fast) for in-place list modification. The slow pointer tracks valid nodes while the fast pointer scans, demonstrating how to filter elements without extra space. This pattern will be extended in Phase 2.

## Phase 2: Advanced Reversal and Group Operations

This phase (5 problems) extends the reversal technique to handle groups, combines reversal with other techniques like interleaving, and introduces partitioning patterns. We learn to process lists in chunks, combine multiple operations, and maintain relative order during partitioning.

- **Problems in this Phase:**
  1. `25. Reverse Nodes in k-Group`
     - **Reasoning:** Extends the reversal pattern to process the list in groups of `k`. Demonstrates repeated application of the three-pointer reversal technique, with careful reconnection between groups. Introduces the concept of processing lists in chunks while maintaining overall structure. This is the generalization of Problem 24 to arbitrary group sizes.
  2. `143. Reorder List`
     - **Reasoning:** Synthesizes multiple techniques: finding the middle (slow/fast pointers), reversing the second half (Problem 206), and interleaving two lists. Demonstrates how reversal can be combined with other operations to achieve complex reordering. Shows that reversal is often a tool in a larger solution, not just an end goal.
  3. `82. Remove Duplicates from Sorted List II`
     - **Reasoning:** Extends the filtering pattern from Problem 203. Instead of removing a single value, we remove all nodes with duplicates, keeping only unique values. Demonstrates how the two-pointer filtering approach can handle more complex conditions. Since the list is sorted, duplicates are adjacent, making the skip-and-connect pattern efficient.
  4. `86. Partition List`
     - **Reasoning:** Introduces the two-list partitioning technique. Creates separate lists for elements meeting different criteria (small vs. big), then concatenates them. Demonstrates how to maintain relative order within partitions while reorganizing the overall structure. The dummy head pattern simplifies edge cases and list construction.
  5. `328. Odd Even Linked List`
     - **Reasoning:** Uses the same two-list partitioning approach as Problem 86, but partitions based on index parity rather than value comparison. Demonstrates that the partitioning pattern is flexible and can work with any classification criteria. Shows how alternating between two lists preserves relative order within each group.

## Phase 3: Reversal as a Tool and Complex Operations

This phase (6 problems) demonstrates using reversal as a problem-solving tool rather than an end goal. We see reversal used to transform problems, enable different traversal directions, and pair nodes from opposite ends. This phase also includes foundational problems for number addition and deep copying with complex pointer relationships.

- **Problems in this Phase:**
  1. `445. Add Two Numbers II`
     - **Reasoning:** Demonstrates using reversal as a transformation technique. Since digits are stored most-significant first, we reverse both lists to convert to the familiar least-significant-first format (Problem 2), perform addition, then reverse the result. Shows that reversal can simplify problems by converting them to known formats.
  2. `2130. Maximum Twin Sum of a Linked List`
     - **Reasoning:** Uses the same approach as Problem 143: find the middle, reverse the second half, then pair corresponding nodes. However, instead of reordering, we calculate sums of twin pairs. Demonstrates how reversal enables pairing nodes from opposite ends for computation, a technique useful for palindrome and symmetry problems.
  3. `2487. Remove Nodes From Linked List`
     - **Reasoning:** Uses reversal to change traversal direction. By reversing the list, we can process from right to left (which becomes left to right after reversal), making it easy to identify nodes with greater values to their right. Demonstrates that reversal can simplify problems that require backward processing by converting them to forward processing.
  4. `1721. Swapping Nodes in a Linked List`
     - **Reasoning:** Uses two pointers with a fixed offset to find nodes by position, then swaps their values. While not strictly a reversal problem, it demonstrates finding nodes relative to the end using pointer offsets. Shows how the two-pointer technique extends beyond reversal to node location and value manipulation.
  5. `138. Copy List with Random Pointer`
     - **Reasoning:** Demonstrates deep copying with complex pointer relationships using a hash map. While not a reversal problem, it showcases sophisticated pointer manipulation required for linked list problems. The two-pass approach (create nodes, then set pointers) ensures all relationships are correctly established. This pattern is essential for problems involving multiple pointer types.
  6. `2. Add Two Numbers`
     - **Reasoning:** The foundational problem for adding numbers represented as linked lists. Since digits are stored in reverse order (least significant first), we can add naturally from left to right. Establishes the standard addition algorithm with carry propagation. Problem 445 builds upon this by using reversal to handle the most-significant-first format.

Together, these three phases take you from basic three-pointer reversal to sophisticated list manipulation, showing how reversal can be both a technique and a problem-solving tool, while also covering essential patterns like filtering, partitioning, and deep copying.
