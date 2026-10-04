# Day 1: Python Foundations for Competitive Programming — From Scratch

Welcome to Day 1 of your competitive programming journey in Python. Unlike systems languages like C++, Python handles memory management and arbitrary-precision integers for you, letting you focus entirely on logic and pattern recognition.

This guide covers everything you need to write lightning-fast, error-free Python code from absolute scratch.

---

## 1. The Anatomy of a Fast Python Competitive Programming Template

In competitive programming, you never use interactive prompts (`input()`) in a loop for large inputs. Instead, you read the entire input stream at once into memory.

Here is your production-grade boilerplate. Save this snippet and commit it to muscle memory:

```python
import sys

# Increase recursion depth safely for tree and graph traversals
sys.setrecursionlimit(300000)


def solve():
    # Read all whitespace-separated tokens from standard input at once
    input_data = sys.stdin.read().split()
    if not input_data:
        return

    # Create an iterator to pull tokens sequentially in O(1) time
    it = iter(input_data)

    # Example: Reading test case count
    num_test_cases = int(next(it))
    results = []

    for _ in range(num_test_cases):
        n = int(next(it))
        # Place your per-test-case logic here
        results.append(str(n))

    # Fast bulk output: Join all string results with newlines and flush once
    sys.stdout.write("\n".join(results) + "\n")


if __name__ == "__main__":
    solve()

```

### Why this structure matters:

* **`sys.stdin.read().split()`**: Pulls all numbers and words into a flat list in a single fast C-level operation, bypassing line-by-line parsing overhead.
* **`iter()` and `next()**`: Allows you to stream through the tokens sequentially without slicing or indexing lists, which avoids $O(N)$ memory copying overhead.
* **`sys.stdout.write`**: Accumulates output in a list and prints it once. Calling `print()` inside a tight loop of $10^5$ iterations will trigger a Time Limit Exceeded (TLE).

---

## 2. Python Data Types & Memory Behavior

Understanding how Python stores and manipulates data prevents subtle runtime bugs during contests.

### Integers: Infinite Precision

Python has **no fixed-width integer overflow limits** (unlike C++'s `int` or `long long`). If a problem requires computing $10^{50} \times 10^{50}$, Python automatically expands memory allocation to hold the exact value:

```python
massive_number = 10**100
print(massive_number * massive_number)  # Computes perfectly without overflow

```

### Division Traps: `/` vs `//`

* **Float Division (`/`)**: Always returns a floating-point number, which introduces IEEE 754 precision errors and turns integers into floats (e.g., `4 / 2` becomes `2.0`). **Never use `/` for array indices or midpoints.**
* **Integer Floor Division (`//`)**: Truncates toward negative infinity and returns a clean integer, making it safe for binary search midpoints and grid coordinates:
```python
mid = (low + high) // 2  # Safe integer arithmetic

```



```

```

---

## 3. Essential Built-in Data Structures & Time Complexities

Knowing the time complexity of Python's built-in operations prevents you from writing code that accidentally runs in $O(N^2)$ time.

| Data Structure | Operation | Average Time Complexity | Worst-Case Time Complexity |
| --- | --- | --- | --- |
| **List (`list`)** | Append / Pop (end) | $O(1)$ | $O(N)$ (due to reallocation) |
|  | Insert / Delete (index) | $O(N)$ | $O(N)$ |
|  | Access / Update by Index | $O(1)$ | $O(1)$ |
| **Set (`set`)** | Membership Check (`in`) | $O(1)$ | $O(N)$ |
|  | Add / Remove Element | $O(1)$ | $O(N)$ |
| **Dictionary (`dict`)** | Key Lookup / Insert | $O(1)$ | $O(N)$ |

### Lists vs. Deque for Queues

If you need a queue where you pop elements from the **front** frequently, **do not use a standard list (`pop(0)`)**. Popping from the front of a list forces Python to shift every remaining element, turning it into an $O(N)$ operation.

Instead, import `deque` from `collections`:

```python
from collections import deque

queue = deque([1, 2, 3])
queue.popleft()  # O(1) time complexity!
queue.append(4)  # O(1) time complexity!

```

---

## 4. Useful Python Shortcuts for Contests

* **Multiple Assignment:** Swap variables or assign coordinates cleanly:
```python
x, y = y, x  # Swap in 1 line

```



```
* **List Comprehensions for 2D Grids:** Initialize a \(N \times M\) grid safely without shared reference bugs:
  ```python
  n, m = 3, 4
  grid = [[0] * m for _ in range(n)]  # Correct independent rows

```

* **Counting Frequencies:** Use `Counter` from `collections` to instantly count elements in an array:
```python
from collections import Counter

arr = [1, 2, 2, 3, 3, 3]
freq = Counter(arr)
print(freq[3])  # Output: 3

```



```

```

---

## Day 1 Practice Checklist

1. **Set Up Your Environment:** Ensure you have Python 3.10+ installed locally along with an editor (VS Code or PyCharm).
2. **Drill Basic Problems:** Go to Codeforces (Div 4 / rating 800) or HackerRank and solve **5 simple implementation problems** using *only* the `sys.stdin.read().split()` boilerplate. Force yourself to banish `input()` from your muscle memory.
