a = {1, 2, 3}
b = {2, 3, 4}

a | b   # union     → {1, 2, 3, 4}
a & b   # intersection → {2, 3}
a - b   # difference  → {1}
a ^ b   # symmetric difference → {1, 4}