# Day 2: Arrays, Strings & Two-Pointer Patterns — Mastering Sequence Manipulation

Welcome to Day 2 of your competitive programming journey in Python. Now that you have your fast I/O boilerplate and basic Python data types locked down, it is time to master sequence manipulation. In competitive programming, arrays (lists) and strings are the canvas for 80% of problems.

This guide covers advanced list manipulation, memory optimization, slicing tricks, and fundamental sequence patterns like Two-Pointers and Prefix Sums.

---

## 1. Python Lists Under the Hood (Dynamic Arrays)

In Python, a `list` is not a fixed-size C-style array; it is a **dynamic array** backed by a contiguous block of memory pointers.

* **How Over-allocation Works:** When you append elements, Python periodically allocates a larger chunk of memory behind the scenes so that future appends are $O(1)$ amortized time.
* **The Pitfall:** Inserting or deleting elements at arbitrary indices (using `insert(index, val)` or `pop(index)`) forces Python to shift memory elements, taking $O(N)$ time. In a loop of $10^5$ operations, this will instantly trigger a Time Limit Exceeded (TLE).

### Common Array Pitfalls to Avoid

* **Mutable Default Arguments / Shared References:** Never initialize multi-dimensional grids using multiplication like `[[0] * m] * n`. This creates $N$ references to the *exact same* list row. Modifying `grid[0][0]` will change the value in every single row!
```python
# ❌ WRONG: Creates shared row references
bad_grid = [[0] * 3] * 3
bad_grid[0][0] = 5
print(bad_grid)  # Output: [[5, 0, 0], [5, 0, 0], [5, 0, 0]]

# ✅ CORRECT: List comprehension creates independent rows
good_grid = [[0] * 3 for _ in range(3)]
good_grid[0][0] = 5
print(good_grid)  # Output: [[5, 0, 0], [0, 0, 0], [0, 0, 0]]

```



---

## 2. Pythonic Slicing & Sub-Array Operations

Python slices (`arr[start:end:step]`) are implemented in C and execute extremely fast. Mastering them saves you from writing tedious manual loops.

* **Reversing a List or String in $O(N)$:**
```python
rev_arr = arr[::-1]

```


* **Copying a List:**
```python
arr_copy = arr[:]  # Shallow copy of the entire list

```


* **Negative Indexing:** Access elements from the end of a sequence without calculating lengths:
```python
last_element = arr[-1]
second_to_last = arr[-2]

```



---

## 3. The Two-Pointer Pattern

When dealing with sorted arrays or searching for pairs/sub-arrays meeting specific criteria, brute-forcing with nested loops takes $O(N^2)$ time. The **Two-Pointer** pattern reduces this to $O(N)$.

### Pattern A: Opposite Ends (Collision Technique)

Used primarily on **sorted arrays** to find pairs that sum to a target, check for palindromes, or solve container problems.

**Example: Two Sum II (Sorted Array)**
Find two numbers in a sorted array that add up to a target sum.

```python
def two_sum_sorted(arr, target):
    left = 0
    right = len(arr) - 1

    while left < right:
        current_sum = arr[left] + arr[right]
        if current_sum == target:
            return (left + 1, right + 1)  # 1-indexed positions
        elif current_sum < target:
            left += 1  # Need a larger sum, move left pointer up
        else:
            right -= 1  # Need a smaller sum, move right pointer down
    return (-1, -1)

```

### Pattern B: Same Direction (Fast & Slow Pointers)

Used for in-place array transformations, removing duplicates, or partitioning data.

**Example: Remove Duplicates from a Sorted Array**
Modify the array in-place such that unique elements come first, and return the new length.

```python
def remove_duplicates(arr):
    if not arr:
        return 0

    slow = 0
    for fast in range(1, len(arr)):
        if arr[fast] != arr[slow]:
            slow += 1
            arr[slow] = arr[fast]

    return slow + 1  # New length of unique elements

```

---

## 4. Prefix Sums: Instant Range Queries

If a problem asks you to calculate the sum of elements between indices $L$ and $R$ multiple times ($Q$ queries), a naive loop takes $O(Q \times N)$.

By precomputing a **Prefix Sum Array**, you can answer any range sum query in **$O(1)$ time** after an $O(N)$ preprocessing step.

```python
# Preprocessing
arr = [1, 2, 3, 4, 5]
n = len(arr)

# prefix[i] stores the sum of elements from index 0 to i-1
prefix = [0] * (n + 1)
for i in range(n):
    prefix[i + 1] = prefix[i] + arr[i]


# Query function: Sum from index L to R inclusive
def range_sum(L, R):
    return prefix[R + 1] - prefix[L]


# Example: sum from index 1 to 3 (elements 2, 3, 4 -> sum = 9)
print(range_sum(1, 3))  # Output: 9

```

---

## Day 2 Practice Checklist

1. **Drill Two-Pointers:** Go to LeetCode or Codeforces and solve **3 problems** involving sorted arrays using the opposite-ends or sliding fast/slow pointers.
2. **Implement Prefix Sums:** Solve a problem that requires range sum queries (e.g., *Subarray Sum Equals K* or basic static range sum queries on CSES/HackerRank) using a prefix sum array to avoid TLE.
