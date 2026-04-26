## Intervals

**What:** Ranges with start and end points. Common in scheduling, maintenance windows, downtime detection.
**Why:** Merging overlapping intervals avoids double-counting and simplifies overlap detection.

**Core pattern — merge overlapping intervals:**
def intervals(lt):
    if not lt:
        raise ValueError("empty input")
    l = sorted(lt)                        # sort by start time — tuples sort by first element
    results = [l[0]]                      # seed with first interval
    for i in range(1, len(l)):
        if results[-1][1] >= l[i][0]:    # overlap: current end >= next start
            results[-1] = (              # merge in place — overwrite last result
                results[-1][0],          # keep earlier start
                max(results[-1][1], l[i][1])  # take later end (handles containment)
            )
        else:
            results.append(l[i])         # no overlap — add as new interval
    return results

**Overlap condition:**
# Two intervals (a, b) and (c, d) overlap when:
b >= c  # end of first >= start of second

**Containment case:**
# (1, 10) and (2, 5) — second is fully inside first
# max(10, 5) = 10 — correctly keeps the wider end

**When to use:**
- "Merge overlapping intervals"
- Maintenance window overlap detection
- Meeting rooms — do any overlap?
- Downtime deduplication

**Gotcha:**
- Sort by start first — otherwise you miss overlaps
- Use sorted() not sort() — avoids mutating caller's list
- results[-1] is always the last merged interval — compare against it, not original list
- max() on end times handles containment — don't assume next end is always larger