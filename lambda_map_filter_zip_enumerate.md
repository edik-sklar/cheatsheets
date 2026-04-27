## Lambda, Map, Filter, Zip, Enumerate

**What:** Functional tools for transforming and combining iterables inline.

**Lambda — anonymous function:**
f = lambda x: x * 2          # single argument
f = lambda x, y: x + y       # multiple arguments
f = lambda x: x % 2 == 0     # returns bool

**Map — apply function to every element:**
result = list(map(lambda x: x * 2, nums))
result = list(map(lambda x: x.strip(), log_lines))  # clean log lines

**Filter — keep elements where function returns True:**
result = list(filter(lambda x: x % 2 == 0, nums))
result = list(filter(lambda x: x > 100, response_times))  # slow requests

**Zip — pair elements from two lists:**
pairs = list(zip(servers, cpu_usage))
# [("web-01", 45), ("web-02", 78), ("web-03", 23)]

# unzip:
servers, cpus = zip(*pairs)

**Enumerate — loop with index:**
for i, alert in enumerate(alerts):
    print(f"{i}: {alert}")

# start index at 1:
for i, alert in enumerate(alerts, start=1):
    print(f"{i}: {alert}")

**Combine them — SRE example:**
# get response times over 200ms with their server names
slow = list(filter(lambda x: x[1] > 200, zip(servers, times)))

**Gotcha:**
map() and filter() return lazy objects — wrap in list() to see results
lambda can only be one expression — no statements, no loops