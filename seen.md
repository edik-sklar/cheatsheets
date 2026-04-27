Duplicate detection:
seen = set()
for num in nums:
    if num in seen:
        return True  # duplicate found
    seen.add(num)

Anagram detection:
return Counter(s1) == Counter(s2)

Sliding window — variable, longest substring no repeats:
seen = {}  # char -> last index
left = 0
for right, ch in enumerate(s):
    if ch in seen and seen[ch] >= left:
        left = seen[ch] + 1
    seen[ch] = right

Memoization:
if n in memo:
    return memo[n]
memo[n] = fib(n-1) + fib(n-2)