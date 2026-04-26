## Sliding Window

**What:** A window (subarray/substring) that moves through data tracking a running calculation.
**Why:** Avoids recalculating from scratch — O(n) instead of O(n²).

**Two types:**
# Fixed window — size K never changes
# Variable window — expands/shrinks based on condition

**Fixed window pattern:**
def max_window(data, k):
    win_sum = sum(data[:k])
    max_sum = win_sum
    for i in range(k, len(data)):
        win_sum += data[i] - data[i - k]  # slide: add right, drop left
        max_sum = max(max_sum, win_sum)
    return max_sum

**Variable window pattern (longest substring no repeats):**
def longest_substring(s):
    if not s:
        return (0, "")
    l, max_length, best_l = 0, 0, 0
    window = {}
    for i, c in enumerate(s):
        if c in window and window[c] >= l:  # real repeat inside window
            l = window[c] + 1
        window[c] = i
        if i - l + 1 > max_length:
            max_length = i - l + 1
            best_l = l
    return max_length, s[best_l:best_l + max_length]

**When to use:**
- Contiguous subarray/substring problem
- "Maximum/minimum sum of K elements"
- "Longest substring with condition"

**Gotcha:**
- Fixed: drop left element when sliding → data[i - k]
- Variable: check window[c] >= l to confirm repeat is inside window, not stale




## Sliding Window UPDATED

**What:** A window (subarray/substring) that moves through data tracking a running calculation.
**Why:** Avoids recalculating from scratch — O(n) instead of O(n²).

**Two types:**
# Fixed window — size K never changes
# Variable window — expands/shrinks based on condition

**Fixed window pattern:**
def max_window(data, k):
    win_sum = sum(data[:k])       # seed with first window
    max_sum = win_sum
    best_left = 0
    best_right = k - 1
    for i in range(k, len(data)):
        win_sum += data[i] - data[i - k]   # slide: add new right, drop old left
        if win_sum > max_sum:
            max_sum = win_sum
            best_left = i - k + 1          # window starts k steps behind i
            best_right = i
    return max_sum, data[best_left:best_right + 1]   # return sum AND elements

**Variable window pattern (longest substring no repeats):**
def longest_substring(s):
    if not s:
        return (0, "")
    l, max_length, best_l = 0, 0, 0
    window = {}                             # tracks char -> last seen index
    for i, c in enumerate(s):
        if c in window and window[c] >= l:  # repeat AND inside current window (not stale)
            l = window[c] + 1              # shrink window — move l past the duplicate
        window[c] = i                       # always update index (overwrite stale or new)
        if i - l + 1 > max_length:         # current window bigger than best so far?
            max_length = i - l + 1         # update best length
            best_l = l                     # snapshot left boundary of best window
    return max_length, s[best_l:best_l + max_length]

**When to use:**
- Contiguous subarray/substring problem
- "Maximum/minimum sum of K elements"
- "Longest substring with condition"

**Gotcha:**
- Fixed: drop left element when sliding → data[i - k]
- Variable: window[c] >= l confirms repeat is inside window, not a stale entry from before l moved
- best_l only updates when a new longest window is found — never shrinks