# Two Pointers Pattern

## Phase 1: The Foundations

This phase (5 problems) establishes the fundamental two-pointer techniques: basic pointer movement, character comparison, element swapping, and conditional pointer advancement. These are the building blocks for all advanced two-pointer problems.

- **Problems in this Phase:**
  1. `167. Two Sum II - Input Array is Sorted`
     - **Reasoning:** The foundational two-pointer problem. Demonstrates the core pattern: start from opposite ends, compare with target, and move pointers based on the comparison. The sorted property enables greedy pointer movement decisions.
  2. `125. Valid Palindrome`
     - **Reasoning:** Applies two pointers to string validation. Shows how to compare characters from both ends and move inward. Introduces the pattern of character-by-character comparison with two pointers.
  3. `344. Reverse String`
     - **Reasoning:** Introduces element swapping with two pointers. Demonstrates in-place array manipulation by swapping elements from opposite ends. This swapping pattern is fundamental to many two-pointer problems.
  4. `977. Squares of a Sorted Array`
     - **Reasoning:** Shows how two pointers work with mathematical properties (squares). Demonstrates comparing absolute values and building a result array from extremes. Introduces the concept of merging/combining elements from opposite ends.
  5. `345. Reverse Vowels of a String`
     - **Reasoning:** Combines swapping (Problem 344) with conditional pointer movement. Introduces the pattern of "skip non-target elements" while maintaining two-pointer approach. This selective processing pattern is crucial for partitioning problems.

## Phase 2: Variations & Extensions

This phase (5 problems) covers variations that extend the basic two-pointer pattern: handling exceptions (one deletion), optimization problems (maximizing area), partitioning (two-way and three-way), and mathematical applications.

- **Problems in this Phase:**
  1. `680. Valid Palindrome II`
     - **Reasoning:** Extends Problem 125 by allowing one deletion. Introduces the concept of backtracking or trying alternatives when standard two-pointer check fails. Demonstrates how to handle exceptions in two-pointer solutions.
  2. `11. Container with Most Water`
     - **Reasoning:** Introduces greedy optimization with two pointers. Unlike previous problems comparing sums, this maximizes area by making intelligent decisions about which pointer to move. Demonstrates greedy decision-making: always move the pointer at the shorter line.
  3. `905. Sort Array By Parity`
     - **Reasoning:** Extends the conditional swapping pattern from Problem 345 to partitioning. Demonstrates two-way partitioning (evens left, odds right) using two pointers with conditional movement. This is the foundation for multi-way partitioning.
  4. `75. Sort Colors`
     - **Reasoning:** Extends Problem 905 to three-way partitioning (Dutch National Flag problem). Introduces three pointers (or two pointers plus iterator) to handle multiple partitions. Demonstrates how the partitioning pattern scales to more groups.
  5. `633. Sum of Square Numbers`
     - **Reasoning:** Combines Problem 167's two-pointer technique with mathematical bounds and square calculations. Shows how two pointers work with mathematical sequences rather than arrays. Demonstrates applying two-pointer technique to equation-solving problems.

## Phase 3: Complex Variations & Synthesis

This phase (5 problems) represents the most sophisticated applications: nested loops with two pointers (K-Sum problems), optimization with closeness metrics, greedy pairing problems, and advanced techniques requiring dynamic tracking of auxiliary information.

- **Problems in this Phase:**
  1. `15. 3Sum`
     - **Reasoning:** Extends Problem 167 to find three numbers summing to zero. Introduces nested loops with two pointers (one outer loop + two-pointer search). Demonstrates duplicate handling at multiple levels, which is essential for K-Sum problems. This is the template for all K-Sum variations.
  2. `16. 3Sum Closest`
     - **Reasoning:** Variation of Problem 15 that optimizes for closeness rather than exact match. Introduces tracking the best solution seen so far while using the same nested two-pointer structure. Demonstrates optimization problems with two pointers.
  3. `18. 4Sum`
     - **Reasoning:** Extends Problem 15 to four numbers, demonstrating double nesting (two outer loops + two pointers). Shows how the pattern scales to higher dimensions and generalizes to K-Sum problems. The nested structure shows the general approach: (K-2) nested loops + two pointers.
  4. `881. Boats to Save People`
     - **Reasoning:** Combines two pointers with greedy algorithm principles for optimization. Unlike previous problems finding specific sums, this minimizes boats by making optimal pairing decisions. Demonstrates greedy pairing: always try to pair heaviest with lightest. Shows how two pointers solve optimization problems beyond just finding matches.
  5. `42. Trapping Rain Water`
     - **Reasoning:** The most sophisticated two-pointer problem. Combines two pointers with dynamic tracking of maximums and requires understanding mathematical structure. Demonstrates asymmetric pointer movement based on local conditions (process the side with smaller maximum). Represents the synthesis of all two-pointer patterns: tracking auxiliary information, intelligent pointer movement decisions, and combining multiple concepts (two pointers + greedy + optimization).
