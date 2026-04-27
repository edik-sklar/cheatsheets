## Recursion — Top Down and Bottom Up

**What:** Solving problems by breaking into smaller subproblems.
**Two parts every recursive function needs:**
1. Base case — stop condition
2. Recursive case — call itself with smaller input

**Top Down — recursion + memoization:**
def fib(n, memo=None):
    if n == 0: return 0
    if n == 1: return 1
    if memo is None: memo = {}
    if n in memo: return memo[n]
    memo[n] = fib(n-1, memo) + fib(n-2, memo)
    return memo[n]

# starts at n, recurses down, caches on the way
# risk: recursion limit (Python default: 1000)

**Bottom Up — iterative, build from base:**
def fib_bottom_up(n):
    fib = [0, 1]
    for i in range(2, n+1):
        fib.append(fib[i-1] + fib[i-2])
    return fib[n]

# starts at 0, builds up to n
# safer — no recursion limit risk

**Bottom Up — O(1) space optimization:**
def fib_optimal(n):
    f0, f1 = 0, 1
    for i in range(2, n+1):
        f0, f1 = f1, f0 + f1
    return f1

**Top Down vs Bottom Up:**
Top Down   → recursive, natural to write, memo dict
Bottom Up  → iterative, faster, no recursion overhead
Both       → O(n) time

**SRE use case:**
- Dependency resolution — what services must start before this one?
- Network path finding — shortest path through hops
- Alert correlation — what triggered what?

**Gotcha:**
Python recursion limit is 1000 — use bottom-up for large inputs
memo=None not memo={} as default — mutable default args are shared across calls