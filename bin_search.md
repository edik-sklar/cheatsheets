## Binary Search

**What:** Finds a target in a sorted list by halving the search space each step.
**Why:** O(log n) — eliminates half the candidates every iteration.

**Key formula — always:**
mid = (right - left) // 2 + left  # NOT right - left // 2 (precedence bug)

**Iterative — preferred in interviews:**
def bin_search_iter(li, n):
    if not li:
        raise ValueError("no list")
    left, right = 0, len(li) - 1
    while left <= right:
        mid = (right - left) // 2 + left
        if li[mid] == n:
            return mid
        elif li[mid] < n:
            left = mid + 1       # target is in right half
        else:
            right = mid - 1      # target is in left half
    return -1                    # not found

**Recursive — wrapper + helper pattern:**
def bin_search(li, n):
    if not li:
        raise ValueError("no list")
    return _bin_search(sorted(li), n)   # sort once here, not in recursive calls

def _bin_search(li, n, l=0, r=None):
    if r is None:
        r = len(li) - 1
    if l > r:                            # base case — search space exhausted
        return -1
    m = (r - l) // 2 + l
    if li[m] == n:
        return m
    elif li[m] < n:
        return _bin_search(li, n, m + 1, r)   # +1 — m already checked
    else:
        return _bin_search(li, n, l, m - 1)   # -1 — m already checked

**When to use:**
- Sorted list, find target index
- "Search in rotated array"
- Finding inflection points — capacity thresholds, first/last occurrence

**Gotcha:**
- mid must be recalculated every iteration — never compute once outside loop
- Move pointers past mid (mid+1, mid-1) — avoids infinite loop on edge cases
- Recursive: sort in wrapper only, not in recursive helper
- Iterative: left <= right (not <) — handles single element lists