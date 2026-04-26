## Heap / Priority Queue

**What:** A list that always keeps the smallest value at index 0. Fast access to min (or max).
**Why:** O(log n) push/pop. Best for top-k problems and priority-based processing.

**Setup:**
import heapq

heap = []                        # empty heap
heapq.heapify(list)              # convert list in place, returns None

**Four key operations:**
heapq.heappush(heap, item)       # push item
heapq.heappop(heap)              # pop smallest
heap[0]                          # peek smallest, no pop
heapq.heapreplace(heap, item)    # pop + push in one step (faster)

**Top-K largest — min heap of size k:**
heap = times[:k]
heapq.heapify(heap)
for time in times[k:]:
    if time > heap[0]:
        heapq.heapreplace(heap, time)
return sorted(heap)

**Process largest first — fake max heap:**
heap = [-i for i in data]        # negate on the way in
heapq.heapify(heap)
while heap:
    val = -heapq.heappop(heap)   # negate on the way out
    process(val)

**Min vs Max:**
- Top-k largest → min heap, no negating (weakest candidate at index 0)
- Process largest first → negate to fake max heap

**Gotcha:**
heapify() returns None — always do in two lines:
heap = [1, 3, 2]
heapq.heapify(heap)   # ✓
heap = heapq.heapify([1,3,2])  # ✗ None