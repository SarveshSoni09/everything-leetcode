# Sliding Window Pattern

## Phase 1: Fixed-Size Windows - The Foundations

This phase (6 problems) establishes the fundamental fixed-size sliding window technique. The core pattern is maintaining a window of constant size `k` and efficiently updating it by removing the leftmost element and adding the rightmost element, achieving O(n) instead of O(n\*k) complexity.

- **Problems in this Phase:**
  1. `643. Maximum Average Subarray I`
     - **Reasoning:** The foundational fixed-size sliding window problem. Demonstrates the core pattern: calculate the first window, then slide by removing the leftmost element and adding the rightmost element. This establishes the fundamental O(n) sliding technique that replaces naive O(n\*k) approaches.
  2. `1876. Substrings of Size Three with Distinct Characters`
     - **Reasoning:** Shows that for very small fixed-size windows, direct comparison can be more efficient than using data structures. Demonstrates the sliding pattern with minimal overhead when the window size is constant and small.
  3. `1456. Maximum Number of Vowels in a Substring of Given Length`
     - **Reasoning:** Extends the fixed-size window pattern to character counting with conditional updates. Introduces the concept of maintaining a running count and updating it incrementally. Demonstrates early exit optimization when the maximum possible value is achieved.
  4. `567. Permutation in String`
     - **Reasoning:** Introduces the frequency map approach for fixed-size sliding windows. Demonstrates how character frequency maps can efficiently check for permutations/anagrams by comparing frequency distributions rather than brute-force comparisons.
  5. `438. Find All Anagrams in a String`
     - **Reasoning:** Extends Problem 567 by collecting all matching indices instead of returning on the first match. Shows how the same frequency map pattern can be adapted to find all occurrences, demonstrating the flexibility of the sliding window approach.
  6. `1343. Number of Sub-arrays of Size K and Average Greater than or Equal to Threshold`
     - **Reasoning:** Demonstrates mathematical transformation to avoid floating-point arithmetic. Shows how `average >= threshold` can be transformed to `sum >= k * threshold`, and extends the pattern to counting qualifying windows rather than finding maximum values.

## Phase 2: Variable-Size Windows - Dynamic Optimization

This phase (4 problems) introduces the variable-size sliding window technique. Unlike fixed-size windows, these windows expand and shrink dynamically based on conditions, requiring careful management of left and right pointers to maintain validity while optimizing for the problem's objective.

- **Problems in this Phase:**
  1. `209. Minimum Size Subarray Sum`
     - **Reasoning:** The foundational variable-size sliding window problem. Introduces the core pattern: expand until a condition is met, then shrink while maintaining the condition to find optimal solutions. This establishes the expand-shrink strategy fundamental to all variable-size window problems.
  2. `3. Longest Substring Without Repeating Characters`
     - **Reasoning:** Applies variable-size windows to character uniqueness constraints. Introduces set-based tracking for duplicates and demonstrates the shrink-until-valid pattern when constraints are violated. Shows how to maintain valid windows by removing conflicting elements from the left.
  3. `1004. Maximum Consecutive Ones III`
     - **Reasoning:** Introduces constraint counting with variable-size windows. Demonstrates tracking a specific constraint (count of zeros) and dynamically shrinking when the constraint is exceeded. Shows how the sliding window pattern applies to binary array optimization problems with limited transformations.
  4. `424. Longest Repeating Character Replacement`
     - **Reasoning:** Combines frequency maps with constraint optimization in variable-size windows. Introduces the concept of calculating required transformations (replacements needed = window_size - max_frequency) and maintaining windows where this value is acceptable. Demonstrates how frequency tracking enables sophisticated constraint satisfaction.

## Phase 3: Advanced Techniques & Synthesis

This phase (4 problems) represents the most sophisticated sliding window applications: distinct element constraints, multiplicative operations, complex validity tracking, and the monotonic deque pattern for extremal value maintenance.

- **Problems in this Phase:**
  1. `904. Fruit into Baskets`
     - **Reasoning:** Applies variable-size sliding window to distinct element constraints. Demonstrates using frequency maps to track distinct types and dynamically shrinking when the number of distinct types exceeds the limit. Shows optimization techniques for avoiding repeated `len()` calls on frequency maps.
  2. `713. Subarray Product Less Than K`
     - **Reasoning:** Introduces sliding windows with multiplicative constraints. Unlike additive constraints, products grow multiplicatively, requiring careful handling. Most importantly, demonstrates the technique of counting all valid subarrays ending at each position (there are `r-l+1` subarrays for a window `[l, r]`), which is a crucial pattern for subarray counting problems.
  3. `76. Minimum Window Substring`
     - **Reasoning:** The most sophisticated variable-size window problem. Synthesizes frequency maps, validity tracking, and optimization objectives. Introduces the `valid` counter pattern to efficiently track when all character requirements are satisfied without expensive map comparisons. Demonstrates the complete expand-until-valid, then shrink-while-valid strategy for finding minimum optimal windows. This represents the culmination of variable-size window techniques.
  4. `239. Sliding Window Maximum`
     - **Reasoning:** Introduces the monotonic deque pattern for fixed-size sliding windows requiring extremal value maintenance. Unlike previous problems, this uses a fixed window but requires efficiently maintaining the maximum. Demonstrates how a deque storing indices in decreasing value order can provide O(n) solution instead of naive O(n\*k). This pattern is essential for efficiently querying maximums/minimums in sliding windows.
