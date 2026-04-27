## Deque (Double-Ended Queue)

**What:** A list you can append and pop from both ends in O(1).
**Why:** Regular list left operations are O(n) — deque fixes that.
**Import:** from collections import deque

**Four key operations:**
d = deque()
d.append(x)        # add right
d.appendleft(x)    # add left
d.pop()            # remove right
d.popleft()        # remove left

**Fixed size — rolling window:**
d = deque(maxlen=5)
d.append(x)        # oldest element drops off left automatically

**SRE use cases:**
- FIFO alert processing — append right, popleft
- Rolling window — last N seconds of metrics, auto-drops old data
- BFS — coming soon

**vs list:**
list.insert(0, x)  # O(n) — shifts everything
d.appendleft(x)    # O(1) — no shifting

**Gotcha:**
deque is not indexable like a list for slicing
d[0] and d[-1] work — d[1:3] does not