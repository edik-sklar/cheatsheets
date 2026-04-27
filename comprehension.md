[expression   for item in iterable   if condition]

expression — what to return for each item
for item in iterable — the loop
if condition — optional filter

## Comprehensions

**What:** Concise way to build lists, sets, dicts, and generators from iterables.

**Four types:**
[x for x in data]              # list
{x for x in data}              # set — unique values
{k: v for k, v in data}        # dict
(x for x in data)              # generator — lazy, one at a time

**With condition:**
[x for x in data if x > 0]                    # filter
{k: v for k, v in d.items() if v > 50}        # dict filter

**With transformation:**
[x.strip().lower() for x in lines]            # clean log lines
[x * 2 for x in nums]                         # transform

**Nested — flatten a 2D list:**
matrix = [[1,2], [3,4], [5,6]]
flat = [x for row in matrix for x in row]
# order matches nested for loops — outer first, inner second

**Dict — flip keys and values:**
flipped = {v: k for k, v in d.items()}

**vs map/filter:**
# these are equivalent:
list(map(lambda x: x*2, nums))
[x*2 for x in nums]              # cleaner

list(filter(lambda x: x>0, nums))
[x for x in nums if x > 0]       # cleaner

**SRE examples:**
# servers with CPU over 50%
{k: v for k, v in metrics.items() if v > 50}

# flatten logs from multiple servers
[log for server_logs in all_logs for log in server_logs]

# unique error codes from log lines
{line.split()[0] for line in lines if "ERROR" in line}

**Gotcha:**
Generator comprehension is lazy — wrap in list() to see all values
Nested order: outer loop first, inner loop second — same as written for loops