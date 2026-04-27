## Stack

**What:** LIFO — last in, first out.
**Use:** DFS, recursion tracking, undo operations, nested structure parsing.

**In Python — just use a list:**
stack = []
stack.append(x)   # push — add to top
stack.pop()       # pop — remove and return top
stack[-1]         # peek — look at top without removing
if not stack:     # check empty

**Queue — FIFO (for comparison):**
from collections import deque
queue = deque()
queue.append(x)    # enqueue — add to right
queue.popleft()    # dequeue — remove from left
queue[0]           # peek front

**When to use which:**
Stack  → DFS, undo, matching brackets, dependency order
Queue  → BFS, alert processing, task scheduling, rate limiting

**SRE example — check balanced brackets in config file:**
stack = []
for ch in config:
    if ch == "{":
        stack.append(ch)
    elif ch == "}":
        if not stack:
            return False
        stack.pop()
return len(stack) == 0

**Gotcha:**
list.pop() removes from the right — correct for stack
list.pop(0) removes from the left — O(n), use deque.popleft() instead