# Day 3: Advanced Strings, Sliding Window & Hashing — Pattern Matching & Substrings

Welcome to Day 3 of your competitive programming journey in Python. After mastering arrays and two-pointers, it is time to tackle strings. Strings appear in over 40% of competitive programming contests, ranging from simple palindrome checks to complex pattern matching and substring optimizations.

This guide covers Python string memory characteristics, sliding window substring patterns, and string hashing fundamentals.

---

## 1. Python Strings Under the Hood (Immutability & Efficiency)

In Python, `str` objects are **immutable sequences** of Unicode characters.

* **The Pitfall of Concatenation:** Because strings cannot be modified in place, doing `s += char` inside a loop of length $N$ creates a brand-new string object at every step, resulting in $O(N^2)$ time complexity and inevitable TLE.
* **The Correct Approach:** Always accumulate characters or substrings in a dynamic list and use `"".join(list_name)` at the end. Joining a list of strings is a fast, $O(N)$ C-level operation:
```python
# ❌ WRONG: O(N^2) time due to string immutability
ans = ""
for char in large_array:
    ans += char

# ✅ CORRECT: O(N) time using a list accumulator
chars = []
for char in large_array:
    chars.append(char)
ans = "".join(chars)

```



---

## 2. Pythonic String Manipulations & ASCII Tricks

Python handles character-to-ASCII conversions effortlessly with built-in functions, which is crucial for frequency counting and hash implementations.

* **Character to ASCII & Back:**
```python
ascii_val = ord('a')  # Output: 97
char_val = chr(97)    # Output: 'a'

```


* **Checking Character Types:**
```python
c = 'A'
c.isalnum()  # True if alphanumeric
c.isalpha()  # True if alphabetic
c.islower()  # True if lowercase

```



### Fixed-Size Frequency Arrays

Instead of heavy dictionary lookups for standard alphabet problems (lowercase English letters `a`-`z`), use a fixed-size integer array for maximum speed:

```python
# O(1) space frequency array for lowercase letters
freq = [0] * 26
for char in "competitive":
    freq[ord(char) - ord('a')] += 1

```

---

## 3. The Sliding Window Pattern

When a problem asks for the longest or shortest **contiguous substring** (or subarray) that satisfies a given condition, a nested brute-force loop takes $O(N^2)$ or $O(N^3)$ time. The **Sliding Window** pattern reduces this to **$O(N)$ time**.

### Variable-Length Sliding Window Template

Used when finding the longest or shortest window that fits a specific constraint (e.g., finding the longest substring with at most $K$ distinct characters).

```python
def longest_substring_k_distinct(s, k):
    from collections import defaultdict
    
    window_counts = defaultdict(int)
    left = 0
    max_len = 0

    for right in range(len(s)):
        # Expand window by including s[right]
        window_counts[s[right]] += 1

        # Contract window from the left if the condition is violated
        while len(window_counts) > k:
            window_counts[s[left]] -= 1
            if window_counts[s[left]] == 0:
                del window_counts[s[left]]
            left += 1

        # Update maximum length found so far
        max_len = max(max_len, right - left + 1)

    return max_len

```

---

## 4. String Hashing: Rolling Hashes & Substring Matching

If a problem asks you to check if a pattern exists in a large text or find duplicate substrings of length $L$ across multiple queries, comparing strings directly takes $O(L)$ per check.

**Polynomial Rolling Hashing** converts any string or substring into an integer hash in $O(1)$ time after an $O(N)$ preprocessing step, allowing instantaneous substring comparisons.

```python
def compute_rolling_hashes(s, base=31, mod=10**9 + 7):
    n = len(s)
    h = [0] * (n + 1)
    p = [1] * (n + 1)

    for i in range(n):
        h[i + 1] = (h[i] * base + ord(s[i])) % mod
        p[i + 1] = (p[i] * base) % mod

    return h, p


# Get hash of substring s[L...R] inclusive in O(1) time
def get_substring_hash(L, R, h, p, mod=10**9 + 7):
    length = R - L + 1
    hash_val = (h[R + 1] - h[L] * p[length]) % mod
    return hash_val

```

---

## Day 3 Practice Checklist

1. **Banish String Concatenation:** Ensure you use `"".join()` exclusively for building strings in your solutions.
2. **Drill Sliding Window:** Go to LeetCode or Codeforces and solve **3 sliding window problems** (e.g., *Longest Substring Without Repeating Characters* or *Minimum Window Substring*).
3. **Practice Hashing:** Solve a string matching or duplicate substring problem using polynomial rolling hashes to avoid TLE.
