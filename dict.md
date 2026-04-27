Core dict methods worth knowing:

pythond = {"ERROR": 2, "INFO": 1}

d.keys()          # dict_keys(["ERROR", "INFO"])
d.values()        # dict_values([2, 1])
d.items()         # dict_items([("ERROR", 2), ("INFO", 1)])
d.get("ERROR", 0) # 2, default if missing
d.update({"WARN": 3})  # merge another dict in
d.pop("INFO")     # removes and returns value
"ERROR" in d      # True — key existence check
fromkeys — creates dict from a list with default value:
pythondict.fromkeys(["ERROR", "INFO", "WARN"], 0)
# {"ERROR": 0, "INFO": 0, "WARN": 0}
Useful when you know your keys upfront.

enumerate — not a dict method, it's for lists:
python
for i, val in enumerate(["a", "b", "c"]):
    print(i, val)  # 0 a, 1 b, 2 c
Gives you index + value simultaneously.

Counter extras:
pythonc.most_common(3)  # top 3
c.update(more)    # add more counts
c1 + c2           # combine two counters


## Dict — Key Operations

**What:** Key-value store. O(1) lookup, insert, delete.
**Import:** none — built in

**Create:**
d = {}
d = {"web-01": 45, "web-02": 78}
d = dict.fromkeys(["web-01", "web-02"], 0)  # all keys same default value

**Read:**
d["web-01"]              # KeyError if missing
d.get("web-01", 0)       # safe read, returns default if missing
"web-01" in d            # check if key exists — True/False

**Write:**
d["web-01"] = 99         # add or overwrite
d.update({"web-01": 99}) # merge — second dict overwrites duplicates
merged = d1 | d2         # merge without mutating (Python 3.9+)

**Delete:**
del d["web-01"]          # remove, no return
d.pop("web-01")          # remove, returns value
d.pop("web-01", None)    # safe pop, no KeyError if missing

**Iterate:**
d.keys()                 # all keys
d.values()               # all values
d.items()                # (key, value) tuples — use this in loops
for k, v in d.items():
    print(k, v)

**defaultdict — no KeyError on missing keys:**
from collections import defaultdict
d = defaultdict(int)     # missing key starts at 0
d = defaultdict(list)    # missing key starts at []
d = defaultdict(set)     # missing key starts at set()

**SRE use cases:**
- Error count per server → defaultdict(int)
- Alerts grouped by severity → defaultdict(list)
- Latest metric per host → regular dict, update() or |
- Safe config lookup → get() with fallback