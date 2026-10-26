# Binary Search Technical Report  

*Prepared by: Technical Writing Team*  
*Date: 26 Oct 2026*  

---  

## Table of Contents  

1. [Introduction](#introduction)  
2. [Algorithm Specification](#algorithm-specification)  
   - 2.1 [Function Signature & Preconditions](#function-signature--preconditions)  
   - 2.2 [Iterative Implementation (Python)](#iterative-implementation-python)  
3. [Step‑by‑Step Walk‑through](#step‑by‑step-walk‑through)  
4. [Example Usages](#example-usages)  
5. [Complexity Analysis](#complexity-analysis)  
   - 5.1 [Time Complexity](#time-complexity)  
   - 5.2 [Space Complexity](#space-complexity)  
   - 5.3 [Summary Table](#summary-table)  
6. [Edge Cases & Practical Tips](#edge-cases--practical-tips)  
7. [Extensions & Variations](#extensions--variations)  
8. [Conclusion](#conclusion)  
9. [References](#references)  

---  

## 1. Introduction  

Binary search is a classic divide‑and‑conquer algorithm for locating a target value inside a **sorted** collection. By repeatedly halving the search interval, it achieves logarithmic time performance, making it the go‑to method for static, ordered data sets.  

This report documents a clean, **iterative** Python implementation, explains its inner workings, provides runnable examples, and analyses its theoretical complexity.  

---  

## 2. Algorithm Specification  

### 2.1 Function Signature & Preconditions  

```python
def binary_search(arr: list, target) -> int:
    """
    Return the index of `target` in a sorted list `arr`,
    or -1 if the target is not present.
    
    Preconditions:
        * `arr` must be sorted in ascending order.
        * Elements of `arr` must be comparable with `target`
          using the <, ==, and > operators.
    """
```

* **Input** – `arr`: a Python list (or any sequence supporting `len` and indexing).  
* **Output** – Integer index (`0 ≤ index < len(arr)`) when the element is found; `-1` otherwise.  

### 2.2 Iterative Implementation (Python)  

```python
def binary_search(arr, target):
    """Return the index of target in a sorted list `arr`, or -1 if not found."""
    left, right = 0, len(arr) - 1          # ①  initialise the search window
    while left <= right:                   # ②  continue while window is valid
        mid = (left + right) // 2          # ③  middle element (integer division)
        if arr[mid] == target:             # ④  success → return index
            return mid
        elif arr[mid] < target:            # ⑤  target lies to the right
            left = mid + 1
        else:                              # ⑥  target lies to the left
            right = mid - 1
    return -1                               # ⑦  loop finished → not present
```

*The algorithm uses only three integer variables (`left`, `right`, `mid`) and runs in constant auxiliary space.*  

---  

## 3. Step‑by‑Step Walk‑through  

| Step | Action | Variable Values (example) | Decision | Reason |
|------|--------|---------------------------|----------|--------|
| ① | Initialise window | `left = 0`, `right = 9` (for a 10‑element list) | – | Whole array is the initial candidate range. |
| ② | Loop condition | `0 ≤ 9` → **True** | – | Continue while there is at least one candidate. |
| ③ | Compute midpoint | `mid = (0 + 9)//2 = 4` | – | Index 4 is the centre of the current window. |
| ④ | Compare `arr[mid]` with `target` | – | If equal → `return mid`. |
| ⑤ | `arr[mid] < target` | – | Move left bound: `left = mid + 1`. |
| ⑥ | `arr[mid] > target` | – | Move right bound: `right = mid - 1`. |
| … | Repeat | Window shrinks roughly by half each iteration. |
| ⑦ | Exit loop (`left > right`) | – | Target never matched → return `-1`. |

**Why the algorithm is O(log n):** each iteration discards half of the remaining elements, so after *k* iterations the candidate set size ≤ *n / 2ᵏ*. The loop stops when this size drops below 1, i.e. when *k ≈ log₂ n*.  

---  

## 4. Example Usages  

### 4.1 Self‑contained script  

```python
def binary_search(arr, target):
    """Return the index of target in a sorted list `arr`, or -1 if not found."""
    left, right = 0, len(arr) - 1
    while left <= right:
        mid = (left + right) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1


if __name__ == "__main__":
    # ----- Example 1: successful search -----
    data = [2, 5, 8, 12, 16, 23, 38, 42, 56, 71]
    print("Array :", data)

    print("Search for 23 →", binary_search(data, 23))   # → 5
    # Expected output: 5 (23 is at index 5)

    # ----- Example 2: unsuccessful search -----
    print("Search for 15 →", binary_search(data, 15))   # → -1
    # Expected output: -1 (15 is not in the list)
```

Running the script (`python binary_search_demo.py`) prints:

```
Array : [2, 5, 8, 12, 16, 23, 38, 42, 56, 71]
Search for 23 → 5
Search for 15 → -1
```

### 4.2 Interactive REPL snippet  

```python
>>> sorted_numbers = [2, 5, 8, 12, 16, 23, 38, 42, 56, 71]
>>> binary_search(sorted_numbers, 23)
5
>>> binary_search(sorted_numbers, 15)
-1
```

---  

## 5. Complexity Analysis  

### 5.1 Time Complexity  

| Scenario | Number of Iterations | Complexity |
|----------|----------------------|------------|
| **Best case** – target is the first midpoint examined | 1 | Θ(1) |
| **Worst case** – target absent or at an extreme end | ⌈log₂ n⌉ | Θ(log n) |
| **Average case** – uniform distribution of target positions (including “not present”) | ≈ log₂ n | Θ(log n) |

**Derivation (recurrence):**  

\[
C(k) = 
\begin{cases}
1, & k = 1\\
1 + C\!\big(\lfloor k/2 \rfloor\big), & k > 1
\end{cases}
\;\Longrightarrow\; C(k) = Θ(\log_2 k)
\]

Thus for an input of size *n*, the worst‑case number of comparisons grows logarithmically.  

### 5.2 Space Complexity  

*Iterative version*: only three integer variables → **O(1)** auxiliary space.  

*Recursive version* (not shown) would require **O(log n)** stack frames, one per recursion level.  

### 5.3 Summary Table  

| Metric | Value |
|--------|-------|
| **Time – Best** | Θ(1) |
| **Time – Average** | Θ(log n) |
| **Time – Worst** | Θ(log n) |
| **Auxiliary Space** | O(1) (iterative) / O(log n) (recursive) |

---  

## 6. Edge Cases & Practical Tips  

| Edge Case | Behaviour | Recommended Handling |
|-----------|------------|----------------------|
| Empty list (`len(arr) == 0`) | `right = -1`; loop never entered → returns `-1`. | No extra code needed. |
| Single‑element list | `mid` = 0; one comparison decides outcome. | Works automatically. |
| Duplicate values | Returns *any* matching index (the one first encountered). | To obtain the **first** occurrence, continue searching left after a match. |
| Unsorted input | May return wrong index or `-1`. | Validate or sort before calling. |
| Large integer indices (in languages with fixed‑size ints) | `mid = (left + right)//2` could overflow. | Use `mid = left + (right - left)//2`. (Python ints are unbounded, but the pattern is portable.) |

**Best‑practice checklist**  

1. **Validate sorting** (or document the requirement clearly).  
2. **Prefer the iterative version** for production code to avoid recursion limits.  
3. **Document the sentinel value** (`-1`) in the API contract.  
4. **Unit‑test edge cases** (empty, single‑element, duplicates, not‑found).  
5. **Consider type hints** (`def binary_search(arr: List[int], target: int) -> int:`) for static analysis tools.  

---  

## 7. Extensions & Variations  

| Variation | Description | When to Use |
|-----------|-------------|-------------|
| **Recursive binary search** | Same logic expressed via function calls. | Educational purposes or when functional style is preferred. |
| **Left‑most / Right‑most occurrence** | After finding a match, continue searching left (or right) to locate the first (or last) duplicate. | Required when the array may contain repeated keys and you need a deterministic position. |
| **Generic comparator** | Pass a custom `key` or comparator function to support complex objects. | Searching in lists of objects (e.g., `namedtuple`, dataclasses). |
| **Iterative with exponential back‑off** | Useful when the size of the sorted data is unknown (e.g., infinite stream). | Searching in unbounded or lazily generated sequences. |
| **Parallel binary search** | Split the array and search sub‑ranges concurrently (rarely needed because log n is already tiny). | Specialized high‑throughput systems where latency is critical and hardware permits parallelism. |

---  

## 8. Conclusion  

Binary search remains one of the most efficient algorithms for exact lookup in a static, sorted collection. The provided Python implementation is:

* **Correct** – returns the exact index or `-1`.  
* **Efficient** – Θ(log n) time, O(1) extra space.  
* **Robust** – handles empty and single‑element arrays gracefully.  

By adhering to the precondition (sorted input) and following the practical tips above, developers can safely integrate binary search into any performance‑sensitive Python code base.  

---  

## 9. References  

1. Cormen, T. H., Leiserson, C. E., Rivest, R. L., & Stein, C. (2022). *Introduction to Algorithms* (4th ed.). MIT Press.  
2. Knuth, D. E. (1998). *The Art of Computer Programming, Volume 3: Sorting and Searching* (2nd ed.). Addison‑Wesley.  
3. Python Software Foundation. (2024). *Python Documentation – Built‑in Types*. https://docs.python.org/3/library/stdtypes.html  

---  

*End of Report*  
