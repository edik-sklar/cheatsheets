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