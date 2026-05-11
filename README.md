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

| Dataset Size | Linear Search | Binary Search | Jump Search | Interpolation Search |
| ------------ | ------------- | ------------- | ----------- | -------------------- |
| 1,000        | 0.002089 ms   | 0.004315 ms   | 0.076887 ms | 0.002004 ms          |
| 10,000       | 0.001208 ms   | 0.001160 ms   | 0.100620 ms | 0.001493 ms          |
| 50,000       | 0.001339 ms   | 0.001498 ms   | 0.219862 ms | 0.002315 ms          |

---

## Search Algorithm Analysis

Based on the benchmark results, binary search demonstrated the most stable and efficient performance overall. At n = 1,000, interpolation search achieved the fastest execution time at 0.002004 ms, slightly outperforming binary search at 0.004315 ms. However, as dataset size increased to 10,000 and 50,000, binary search became consistently faster, requiring only 0.001160 ms and 0.001498 ms respectively.

Jump search produced the slowest performance across all dataset sizes, reaching 0.219862 ms at n = 50,000 due to the additional jumping and scanning operations required before locating the target value.

Although linear search occasionally produced very small measured times, its overall complexity remains O(n), making it less scalable for significantly larger datasets.

---|---|---|---|---|
| 1,000 | 0.002089 ms | 0.004315 ms | 0.076887 ms | 0.002004 ms |
| 10,000 | 0.001208 ms | 0.001160 ms | 0.100620 ms | 0.001493 ms |
| 50,000 | 0.001339 ms | 0.001498 ms | 0.219862 ms | 0.002315 ms |

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

| Method             | Time         |
| ------------------ | ------------ |
| BST Range Query    | 0.6491 ms    |
| Baseline Filtering | 1330.2251 ms |

---

## BST Analysis

The BST range query significantly outperformed the baseline filtering approach. At n = 50,000, the BST query required only 0.6491 ms, while the baseline sequential filtering operation required 1330.2251 ms.

The BST was more efficient because the tree structure allowed the algorithm to skip irrelevant branches and traverse only nodes within the requested range. In contrast, the baseline method scanned every row in the dataset sequentially.

The results demonstrate how ordered tree structures can dramatically reduce unnecessary comparisons during range-based queries.

---|---|
| BST Range Query | 0.6491 ms |
| Baseline Filtering | 1330.2251 ms |

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

| Method           | Time         |
| ---------------- | ------------ |
| Heap Top-k       | 2689.0427 ms |
| Baseline Sorting | 189.5159 ms  |

---

## Heap Analysis

Although heaps are theoretically efficient for top-k queries, the baseline sorting implementation performed faster in practice. The custom heap implementation required 2689.0427 ms, while Python's built-in sorting required only 189.5159 ms.

This occurred because Python's `sorted()` function is highly optimized internally in C, whereas the manually implemented heap introduced additional Python-level overhead through repeated swaps, heapify operations, and row conversions.

Despite the slower runtime, the heap implementation still correctly maintained the top 10 most popular songs while processing the dataset incrementally.

---|---|
| Heap Top-k | 2689.0427 ms |
| Baseline Sorting | 189.5159 ms |

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
