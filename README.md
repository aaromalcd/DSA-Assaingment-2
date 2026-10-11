# Hospital Priority Queue: Max Heap, Heap Sort and Quick Sort

A hospital manages patients using a priority queue, where a **higher severity score means higher priority**. This project implements a Max Heap, Heap Sort and Quick Sort in Python and compares them.

**Patient severity scores:** `45, 72, 30, 90, 65, 50, 85`

## Files

| File | Description |
|------|-------------|
| `priority_queue.py` | Max Heap insertion, Heap Sort, Quick Sort (with step tracing and counters) |
| `output.txt` | Saved output of one execution |
| `README.md` | This report |

## How to run

```bash
python3 priority_queue.py
```

Requires Python 3 only (no external libraries).

---

## a) Max Heap: heap after each insertion

Each new score is added at the end and moved up (swapped with its parent) while it is larger than its parent.

| Insert | Heap (array form) |
|-------:|-------------------|
| 45 | `[45]` |
| 72 | `[72, 45]` |
| 30 | `[72, 45, 30]` |
| 90 | `[90, 72, 30, 45]` |
| 65 | `[90, 72, 30, 45, 65]` |
| 50 | `[90, 72, 50, 45, 65, 30]` |
| 85 | `[90, 72, 85, 45, 65, 30, 50]` |

**Final heap tree:**

```
            90
         /      \
       72        85
      /  \      /  \
    45    65  30    50
```

- Highest priority patient (root): **90**
- Height of heap: **2**
- Insertion cost: **9 comparisons, 5 swaps**

---

## b) Sorting the severity scores

### Heap Sort

**Phase 1: build max heap**

```
heapify at index 2 -> [45, 72, 85, 90, 65, 50, 30]
heapify at index 1 -> [45, 90, 85, 72, 65, 50, 30]
heapify at index 0 -> [90, 72, 85, 45, 65, 50, 30]
```

**Phase 2: extract maximum repeatedly**

```
move 90 to position 6 -> [85, 72, 50, 45, 65, 30, 90]
move 85 to position 5 -> [72, 65, 50, 45, 30, 85, 90]
move 72 to position 4 -> [65, 45, 50, 30, 72, 85, 90]
move 65 to position 3 -> [50, 45, 30, 65, 72, 85, 90]
move 50 to position 2 -> [45, 30, 50, 65, 72, 85, 90]
move 45 to position 1 -> [30, 45, 50, 65, 72, 85, 90]
```

- Sorted (ascending): `[30, 45, 50, 65, 72, 85, 90]`
- Treatment order (highest first): `[90, 85, 72, 65, 50, 45, 30]`
- **21 comparisons, 18 swaps**

### Quick Sort (last element as pivot)

```
pivot=85 range[0..6] -> [45, 72, 30, 65, 50, 85, 90]
  pivot=50 range[0..4] -> [45, 30, 50, 65, 72, 85, 90]
    pivot=30 range[0..1] -> [30, 45, 50, 65, 72, 85, 90]
    pivot=72 range[3..4] -> [30, 45, 50, 65, 72, 85, 90]
```

- Sorted (ascending): `[30, 45, 50, 65, 72, 85, 90]`
- Treatment order (highest first): `[90, 85, 72, 65, 50, 45, 30]`
- **12 comparisons, 6 swaps**

---

## c) Analysis

| Criterion | Max Heap / Heap Sort | Quick Sort |
|-----------|----------------------|------------|
| **Structure and height** | Complete binary tree stored in an array. Height = ⌊log₂ n⌋ = **2** for 7 patients. Always balanced. | No persistent structure. Recursion depth was **3** here, but it depends on pivot choices and can reach n in the worst case. |
| **Comparisons / swaps observed** | Insertion only: 9 comparisons, 5 swaps. Full Heap Sort: 21 comparisons, 18 swaps. | 12 comparisons, 6 swaps. |
| **Time complexity** | Insert: O(log n). Get max: O(1). Remove max: O(log n). Heap Sort: O(n log n) in all cases. | Average O(n log n), worst case O(n²). Re-sorting after every insertion costs O(n log n) each time. |
| **Space** | O(1) extra (in place) | O(log n) average for the recursion stack, O(n) worst case |

Quick Sort used fewer comparisons and swaps on this small fixed list, which is its usual strength for a one-time sort. Heap Sort does more work here because it fully sorts the data by extracting every element, which is more than a priority queue needs.

### Conclusion: which suits the hospital?

The **Max Heap (priority queue)** is the better choice when patients keep arriving and the highest-priority patient is needed immediately:

- The most severe patient is always at the root, so it is available in **O(1)**.
- Each new patient is inserted in **O(log n)**, and each treated patient is removed in **O(log n)**.
- The heap is never fully re-sorted, so no work is repeated.
- Quick Sort needs the whole list again after every new arrival (O(n log n) each time), and it has an O(n²) worst case, which is risky for a medical system.

Quick Sort is better only when the full list is available once and needs to be sorted in one go.
