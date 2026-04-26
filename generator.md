## Generators

**What:** Produces values one at a time on demand. Lazy list.
**Why:** Memory efficient for large data — one item in memory at a time.

**Three ways to create:**
# 1 - generator expression
g = (x for x in data)

# 2 - yield function  
def gen(data):
    for x in data:
        yield x

# 3 - convert list
g = iter([1, 2, 3])

**How to consume:**
next(g)          # manually, one at a time
for x in g:      # automatically calls next() until StopIteration

**Use list instead when:**
- Need len(), indexing, or multiple iterations

**Gotcha:**
g = (x for x in [1,2,3])
list(g)  # [1,2,3]
list(g)  # [] ← exhausted after one pass