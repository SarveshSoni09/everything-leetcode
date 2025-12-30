# Top K Elements Pattern

## Phase 1: Foundation - Min/Max Heaps for Selection

This phase (5 problems) establishes the fundamental heap-based approach for finding top K elements. We learn to use min heaps for "Kth largest" problems, max heaps (simulated with min heaps using negatives) for "Kth smallest" and frequency-based problems, and how to maintain heap size efficiently.

- **Problems in this Phase:**
  1. `215. Kth Largest Element in an Array`
     - **Reasoning:** The foundational problem for the Top K pattern. Introduces the key insight: maintain a min heap of size `k` to find the `kth` largest element. The heap stores the `k` largest elements seen so far, with the smallest of these (the `kth` largest) at the root. This establishes the core technique of size-limited heaps for efficient selection.
  2. `703. Kth Largest Element in a Stream`
     - **Reasoning:** Extends Problem 215 to handle streaming data. Demonstrates how the same min heap approach applies when elements arrive incrementally. Shows that maintaining a heap of size `k` across multiple operations is efficient, establishing the pattern for stream processing problems. The heap is updated dynamically as new elements arrive.
  3. `973. K Closest Points to Origin`
     - **Reasoning:** Applies the heap pattern to distance-based selection. Introduces using max heap (simulated with negative distances) to find the `k` closest points. Demonstrates how custom comparison functions (distance calculations) can be integrated into the heap pattern. Shows that the same size-`k` heap technique works for any metric-based selection.
  4. `347. Top K Frequent Elements`
     - **Reasoning:** Extends the heap pattern to frequency-based problems. Introduces the frequency counting step (using Counter) followed by heap-based selection. Demonstrates using max heap (negative frequencies) to select top K by frequency. Establishes the two-step pattern: count frequencies, then heap-select, which is fundamental to frequency-based Top K problems.
  5. `692. Top K Frequent Words`
     - **Reasoning:** Extends Problem 347 to handle tie-breaking with lexicographical ordering. Demonstrates how Python's tuple comparison naturally handles multi-criteria sorting (frequency first, then alphabetical order). Shows that the heap pattern can accommodate complex comparison criteria by carefully structuring heap elements as tuples.

## Phase 2: Leveraging Sorted Properties & Merging

This phase (5 problems) builds on the foundation by optimizing heap usage when input data has sorted properties, and introduces heap-based merging techniques. We learn to reduce heap size from O(n) to O(k) by processing sorted structures incrementally, and see how heaps efficiently merge multiple sorted sequences.

- **Problems in this Phase:**
  1. `451. Sort Characters by Frequency`
     - **Reasoning:** Extends the frequency-based heap pattern to sort all elements by frequency, not just select top K. Demonstrates that the heap naturally sorts when we pop all elements. Shows how the same frequency counting and heap pattern can be used for full sorting tasks, bridging selection and sorting problems.
  2. `378. Kth Smallest Element in a Sorted Matrix`
     - **Reasoning:** Introduces leveraging sorted properties to reduce heap size. The brute force approach treats the matrix as a 1D array, but the optimized approach maintains only one element per row in the heap (size O(k) instead of O(n²)). Demonstrates how sorted structures allow incremental processing, reducing both time and space complexity.
  3. `767. Reorganize String`
     - **Reasoning:** Introduces greedy heap-based construction with constraint satisfaction. Uses a max heap to always select the most frequent available character, while maintaining a "previous character" to avoid violations. Demonstrates how heaps enable greedy strategies for construction problems, and shows the pattern of maintaining state (prev) alongside the heap for constraint handling.
  4. `23. Merge k Sorted Lists`
     - **Reasoning:** Establishes the heap-based merge pattern for multiple sorted sequences. The key insight is maintaining only the current "head" of each list in the heap, reducing space from O(N) to O(k). Demonstrates how heaps efficiently merge multiple sorted sequences by always processing the smallest available element. This pattern is fundamental for external sorting and multi-sequence merging.
  5. `373. Find K Pairs with Smallest Sums`
     - **Reasoning:** Extends the sorted structure optimization from Problem 378 to pairs. Demonstrates tracking indices in sorted arrays to maintain a heap of size O(k) instead of generating all pairs. Shows how the sorted property enables incremental pair generation, similar to how Problem 378 processes matrix rows incrementally. This pattern applies whenever we need top K combinations from sorted inputs.

## Phase 3: Advanced Heap Applications & Greedy Strategies

This phase (5 problems) demonstrates sophisticated heap applications: greedy construction with heaps, two-heap techniques for medians, event processing with multiple heaps, and time-based selection. These problems show heaps as a versatile tool for complex algorithmic patterns beyond simple selection.

- **Problems in this Phase:**
  1. `659. Split Array into Consecutive Subsequences`
     - **Reasoning:** Introduces greedy heap-based construction with multiple valid extensions. Uses min heaps to track subsequence lengths, always extending the shortest valid subsequence. Demonstrates how heaps enable greedy strategies for grouping/partitioning problems, and shows the pattern of maintaining heaps keyed by different criteria (ending value, length).
  2. `295. Find Median from Data Stream`
     - **Reasoning:** Introduces the two-heap technique for maintaining running medians. Uses a max heap for the smaller half and a min heap for the larger half, keeping them balanced. Demonstrates how two coordinated heaps can maintain order statistics efficiently. This pattern extends beyond medians to any percentile or order statistic maintenance.
  3. `355. Design Twitter`
     - **Reasoning:** Combines heap-based merging (Problem 23) with time-based ordering. Demonstrates merging multiple sorted sequences (user tweet lists) where each sequence is sorted by time. Shows how maintaining one pointer per sequence in a heap enables efficient merging of time-ordered streams. This pattern applies to any multi-source feed aggregation problem.
  4. `1094. Car Pooling`
     - **Reasoning:** Introduces event processing with multiple heaps. Uses separate heaps for pickup and dropoff events, processing them chronologically. Demonstrates how heaps enable efficient timeline simulation by always processing the next event. Shows the pattern of coordinating multiple heaps for event-driven problems.
  5. `1705. Maximum Number of Eaten Apples`
     - **Reasoning:** Demonstrates greedy consumption with expiration-based priority. Uses a min heap keyed by expiration day to always consume the apple closest to rotting. Shows how heaps enable greedy strategies for consumption/scheduling problems with deadlines. Demonstrates the pattern of maintaining items with time-based priorities and removing expired items before processing.

Together, these three phases take you from basic heap-based selection to sophisticated applications where heaps coordinate multiple data structures, enable greedy strategies, and efficiently process time-ordered events, establishing heaps as a fundamental tool for optimization and selection problems.
