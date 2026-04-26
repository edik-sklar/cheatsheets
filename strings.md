## Strings

**Key fact:** Immutable, iterable, indexable

**Slicing:**
s = "ERROR 2024-01-01"
s[0:5]    # "ERROR"
s[-8:]    # "01-01" — negative index from end
s[::-1]   # reverse string

**Common methods:**
s.split()         # ["ERROR", "2024-01-01"] — splits on whitespace
s.split(",")      # split on specific char
",".join(["a","b","c"])  # "a,b,c" — opposite of split
s.strip()         # remove whitespace both ends
s.rstrip("0")     # remove trailing zeros — our first problem!
s.upper()         # "ERROR 2024-01-01"
s.lower()         # "error 2024-01-01"
s.startswith("ERROR")  # True
s.endswith("01")       # True
s.replace("ERROR", "WARN")  # "WARN 2024-01-01"

**Checking content:**
"ERROR" in s      # True — O(n) scan
s.split()[0]      # extract first word

**f-strings:**
name = "Edik"
f"Hello {name}"   # "Hello Edik"
f"Sum is {1+2}"   # "Sum is 3"

**Gotcha:**
s = "hello"
s[0] = "H"  # ❌ TypeError — strings are immutable
s = "H" + s[1:]  # ✅ create new string instead