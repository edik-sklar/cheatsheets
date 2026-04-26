## Slicing

**Syntax:** s[start:stop:step]
- start — inclusive
- stop — exclusive  
- step — how many to skip

s = "ERROR 2024-01-01"
#    0123456789...

**Basic:**
s[0:5]    # "ERROR"   — index 0 to 4
s[6:]     # "2024-01-01" — from 6 to end
s[:5]     # "ERROR"   — from start to 4
s[:]      # full copy

**Negative index — count from end:**
s[-1]     # "1"  — last char
s[-8:]    # "24-01-01" — last 8 chars...

# better example:
s = "ERROR"
s[-3:]    # "ROR" — last 3 chars
s[:-2]    # "ERR" — all except last 2

**Step:**
s[::2]    # every other char
s[::-1]   # reverse — step -1 goes backwards

**Lists use same rules:**
requests[2:5]   # elements at index 2,3,4
requests[-3:]   # last 3 elements
requests[::-1]  # reversed list

**Gotcha:**
s[0:5]  # includes 0, excludes 5
s[:3]   # same as s[0:3]
# when in doubt — trace with a short concrete example!