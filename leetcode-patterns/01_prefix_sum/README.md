# Prefix Sum Pattern

## Phase 1: The Foundations (1D)

This phase (5 problems) establishes the fundamental concepts: what a prefix array is, why it's useful for $O(1)$ queries, and the introduction of the Prefix Sum + Hash Map pattern, which is the most important concept in this entire set.

- **Problems in this Phase:**
  1. `1480. Running Sum of 1D Array`
     - **Reasoning:** The literal definition of a prefix sum. This is step zero.
  2. `303. Range Sum Query - Immutable`
     - **Reasoning:** The purpose of a prefix sum. Shows why we build it: to get $O(1)$ range queries.
  3. `1310. XOR Queries of a Subarray`
     - **Reasoning:** Generalizes the pattern. It proves that this isn't just for "sum," but for any operation with an associative inverse (like XOR).
  4. `560. Subarray Sum Equals K`
     - **Reasoning:** The most important conceptual leap. Introduces the Prefix Sum + Hash Map pattern for counting subarrays. The logic `P[j] - P[i-1] = k becomes P[i-1] = P[j] - k`.
  5. `325. Maximum Size Subarray Sum Equals K`
     - **Reasoning:** A crucial variation of 560. The hash map now stores the first seen index instead of a frequency count to find the maximum possible length.

## Phase 2: 1D Variations & Disguises

This phase (5 problems) covers problems that are "disguised" versions of Phase 1 problems. The core logic is identical, but you first need to transform the input or apply a new idea (like modular arithmetic).

- **Problems in this Phase:**
  1. `930. Binary Subarray with Sum`
     - **Reasoning:** A "disguised" Problem 560. It's a binary array, but the logic is identical.
  2. `1248. Count Number of Nice Subarrays`
     - **Reasoning:** Another "disguised" Problem 560. The trick is to transform the array (odd -> 1, even -> 0), after which it's exactly Problem 560.
  3. `525. Contiguous Subarray`
     - **Reasoning:** A "disguised" Problem 325. You transform the array (0 -> -1, 1 -> 1) and then solve for "max length subarray with sum 0."
  4. `523. Continuous Subarray Sum`
     - **Reasoning:** Introduces a new variation: modular arithmetic. The hash map key becomes `prefix_sum % k` and the value is the `index` (like Problem 325).
  5. `974. Subarray Sums Divisible by K`
     - **Reasoning:** The final 1D synthesis. It combines Problem 560 (counting) with Problem 523 (modular arithmetic). The hash map stores `{remainder: frequency}`.

## Phase 3: 2D & Complex Applications

This phase (3 problems) takes all the 1D concepts and applies them to 2D matrices. This introduces 2D prefix sum (inclusion-exclusion) and the complex "2D-to-1D" reduction technique.

- **Problems in this Phase:**
  1. `304. Range Sum Query 2D - Immutable`
     - **Reasoning:** The 2D foundation. Extends Problem 303 to 2D, introducing the 2D inclusion-exclusion principle for $O(1)$ query.
  2. `1314. Matrix Block Sum`
     - **Reasoning:** A direct application of Problem 304. You build the 2D prefix sum matrix once, and then run $O(mn)$ queries on it (one for each cell) to get the final answer.
  3. `1074. Number of Submatrices That Sum to Target`
     - **Reasoning:** The "final boss" of this set. This problem is not a simple 2D query. It's a 2D-to-1D reduction. You iterate all $O(m^2)$ row-pairs, "squishing" the matrix into a 1D array, and then run the 1D solution (Problem 560) on it $O(m^2)$ times.
