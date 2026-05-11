# Data Structures and Algorithms — Lab 3

## Overview

This project explores the implementation, benchmarking, and analysis of several classical data structures and searching techniques using a real-world music dataset containing more than 50,000 rows.

The notebook focuses on:

* Search algorithms
* Binary Search Trees (BST)
* Heap-based Top-k queries
* Performance benchmarking and complexity analysis
* Comparison against naive baseline approaches

All algorithms were implemented from scratch without using built-in search utilities such as `bisect`, `heapq`, or Python membership operators.

---

# Dataset

## Topic

Music dataset containing song metadata and popularity information.

## Dataset Characteristics

* More than 50,000 rows
* Includes track titles, artists, popularity scores, and additional metadata
* Used for:

  * exact lookup queries
  * ordered BST queries
  * top-k popularity queries
  * range queries

---

# Technologies Used

* Python
* Pandas
* NumPy
* Matplotlib
* Plotly
* Jupyter Notebook

---

# Task 1 — Search Algorithms

## Goal

Implement and compare four search algorithms:

1. Linear Search
2. Binary Search
3. Jump Search
4. Interpolation Search

The algorithms were benchmarked on datasets of size:

* 1,000
* 10,000
* 50,000

Each benchmark was repeated three times using `time.perf_counter()` and averaged.

---

## Implemented Algorithms

### Linear Search

Sequentially scans every element until the target is found.

### Binary Search

Repeatedly divides the sorted dataset in half.

### Jump Search

Skips blocks of elements before performing a smaller linear search.

### Interpolation Search

Estimates the likely position of the target based on value distribution.

---

## Search Benchmark Results

| Dataset Size | Binary Search | Interpolation Search | Jump Search | Linear Search |
| ------------ | ------------- | -------------------- | ----------- | ------------- |
| 1,000        | 0.001963 ms   | 0.002690 ms          | 0.054423 ms | 0.002467 ms   |
| 10,000       | 0.001733 ms   | 0.001413 ms          | 0.157970 ms | 0.001983 ms   |
| 50,000       | 0.001904 ms   | 0.001644 ms          | 0.282153 ms | 0.002376 ms   |

---

## Search Algorithm Analysis

Binary Search and Interpolation Search showed the best overall performance. At n = 10,000, Interpolation Search took approximately 0.0014 ms, while Binary Search took 0.0017 ms. Linear Search remained competitive on small datasets at 0.0019 ms for 10,000 elements, while Jump Search consistently performed the worst, increasing from 0.0544 ms at 1,000 elements to 0.2821 ms at 50,000 elements.

These results reflect their theoretical complexities, with Binary Search and Interpolation Search outperforming algorithms with higher complexity as the dataset size increased.

---

## Observations

* Binary search demonstrated the most stable and efficient performance overall.
* Interpolation search slightly outperformed binary search at smaller dataset sizes.
* Jump search consistently produced the slowest execution times.
* Search performance differences remained small because lookup operations are extremely fast in Python.

---

# Task 2 — Binary Search Tree (BST)

## Goal

Implement a Binary Search Tree from scratch supporting:

* insertion
* search
* deletion
* traversals
* breadth-first traversal
* range queries

---

## BST Operations Implemented

### insert(key, value)

Adds nodes recursively while preserving BST ordering rules.

### search(key)

Searches recursively through left and right subtrees.

### delete(key)

Correctly handles:

1. Leaf nodes
2. Nodes with one child
3. Nodes with two children

### Traversals

* inorder()
* preorder()
* postorder()
* bfs()

---

## Range Query

A range query retrieves all values where:

```python
low <= key <= high
```

The BST implementation avoids traversing unnecessary branches, improving efficiency.

---

## BST Benchmark Results

| Method             | Time       |
| ------------------ | ---------- |
| BST Range Query    | 0.95 ms    |
| Baseline Filtering | 1785.96 ms |

---

## BST Analysis

The BST range query significantly outperformed the baseline linear scan. For 50,000 elements, the BST query completed in approximately 0.95 ms, while the baseline filter required 1785.96 ms.

The BST performs faster because it skips subtrees outside the requested range instead of scanning every row sequentially.

The results demonstrate how ordered tree structures can dramatically reduce unnecessary comparisons during range-based queries.

---

## Observations

The BST range query significantly outperformed the baseline filtering approach.

The BST was more efficient because:

* irrelevant branches were skipped
* only relevant subtrees were traversed
* the baseline scanned every row sequentially

---

# Task 3 — Heap and Top-k Query

## Goal

Implement a heap manually using arrays and use it to solve a top-k query.

Top-k query used:

```text
Find the 10 most popular songs
```

A min-heap of size 10 was used.

---

## Heap Operations Implemented

### insert(value)

Adds a new element and restores heap order using heapify-up.

### extract()

Removes the smallest element and restores heap order using heapify-down.

### peek()

Returns the root element without removing it.

---

## Heap Benchmark Results

| Method           | Time       |
| ---------------- | ---------- |
| Heap Top-k       | 2840.41 ms |
| Baseline Sorting | 197.44 ms  |

---

## Heap Analysis

The baseline sorting approach outperformed the custom Heap Top-k implementation. At 50,000 elements, sorting completed in approximately 197.44 ms, while the heap implementation required around 2840.41 ms.

Although heaps have better theoretical complexity for top-k problems, Python’s built-in `sorted()` function is highly optimized in C, making it faster in practice 

---

## Observations

Although heaps are theoretically efficient for top-k problems, the baseline sorting approach performed faster in practice.

This occurred because:

* Python's built-in `sorted()` is highly optimized internally in C
* custom heap operations introduced additional Python-level overhead
* repeated swaps and heapify operations increased execution time

---

# Task 4 — Synthesis and Reflection

## Overall Findings

### Search Algorithms

Binary search demonstrated the most stable performance across increasing dataset sizes.

Interpolation search performed well on smaller datasets due to value distribution assumptions but became slightly slower at larger scales.

Jump search consistently showed the slowest performance because it combines jumping operations with additional linear scans.

---

### BST Performance

BST range queries significantly outperformed sequential filtering.

The BST structure efficiently narrowed the search space and avoided scanning irrelevant values.

---

### Heap Performance

The custom heap implementation did not outperform Python's built-in sorting implementation in practice.

Despite favorable theoretical complexity, implementation overhead in pure Python affected performance.

---

# Degenerate BST Experiment

A BST was constructed using sorted input:

```python
1, 2, 3, 4, ..., 1000
```

This produced a highly unbalanced tree resembling a linked list.

Consequences:

* tree depth increased dramatically
* operations degraded toward linear complexity
* search efficiency worsened significantly

A better tree structure would maintain balance between left and right subtrees.

Examples include:

* AVL Trees
* Red-Black Trees

These structures maintain near-logarithmic height automatically.

---

# Complexity Analysis

| Algorithm / Structure | Average Complexity   |
| --------------------- | -------------------- |
| Linear Search         | O(n)                 |
| Binary Search         | O(log n)             |
| Jump Search           | O(√n)                |
| Interpolation Search  | O(log log n) average |
| BST Search            | O(log n) average     |
| Heap Insert           | O(log k)             |
| Heap Extract          | O(log k)             |

---

# Conclusion

This project demonstrated how algorithmic design and data structure selection significantly affect performance.

Key conclusions:

* Binary search is highly efficient for sorted datasets.
* BST structures enable efficient range queries.
* Heaps are theoretically well-suited for top-k problems.
* Real-world performance depends not only on theoretical complexity but also on implementation details and language optimizations.

The project also highlighted the importance of balancing trees and selecting data structures according to the problem requirements.

---

# Repository Structure

```text
lab3/
│
├── lab3_notebook.ipynb
├── dataset.csv
└── README.md
```

---

# Author

Mariia Cheprakova

Data Structures and Algorithms — 2026
