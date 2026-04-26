## Two Pointers

**What:** Two indices moving through a sorted list to find pairs/triplets that meet a condition.
**Why:** Eliminates candidates at each step — O(n) instead of O(n²) brute force.

**Two modes:**
# Same direction — both move right (deduplication)
# Opposite direction — move toward each other (pair/triplet search)

**Same direction — remove duplicates:**
def remove_dup(nums):
    if not nums:
        raise ValueError("empty list")
    l, r = 0, 1
    results = []
    while r < len(nums):
        if nums[l] == nums[r]:
            r += 1                        # skip duplicate, expand right
        else:
            results.append(nums[l])       # new unique value found
            l = r                         # move l to current r
            r += 1
    results.append(nums[l])              # last element never caught in loop
    return results

**Opposite direction — zero pair:**
def zero_pair(nums):
    if not nums:
        raise ValueError("empty")
    nums = sorted(nums)                  # MUST be sorted — decisions depend on order
    l, r = 0, len(nums) - 1
    results = []
    while l < r:
        if nums[l] + nums[r] == 0:
            results.append((nums[l], nums[r]))
            l += 1
            r -= 1                       # found match, move both inward
        elif nums[l] + nums[r] < 0:
            l += 1                       # sum too small, move left up
        else:
            r -= 1                       # sum too big, move right down
    return results

**3Sum — fix one, two pointers on the rest:**
def three_sum(nums):
    if not nums:
        raise ValueError("empty list")
    nums = sorted(nums)
    results = []
    for current in range(len(nums) - 2):  # stop 2 from end — need room for l and r
        left = current + 1               # l starts just right of current
        right = len(nums) - 1            # r starts at end
        while left < right:
            total = nums[current] + nums[left] + nums[right]
            if total == 0:
                results.append((nums[current], nums[left], nums[right]))
                left += 1
                right -= 1               # keep searching — more pairs may exist
            elif total < 0:
                left += 1
            else:
                right -= 1
    return results

**When to use:**
- Sorted list — pair or triplet that meets a condition
- Remove duplicates without extra memory
- "Two sum on sorted array", "3sum", "container with most water"

**Gotcha:**
- Always sort first if not guaranteed sorted — use sorted() not sort() to avoid mutating caller's list
- Same direction: last element needs manual append after loop
- Opposite direction: l < r stops them from crossing
- 3Sum: move BOTH pointers after a match or you infinite loop