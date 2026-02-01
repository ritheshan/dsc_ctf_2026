# Coding Challenges Cheatsheet

## Python Essentials

### Pwntools for Netcat

```python
from pwn import *

conn = remote('host', port)
conn.recvuntil(b':')
conn.sendline(b'answer')
print(conn.recvall().decode())
```

### Fast I/O

```python
import sys
input = sys.stdin.readline
print = sys.stdout.write
```

### Eval Math Expressions

```python
# Safe eval for math
expr = "2 + 3 * 4"
result = eval(expr)

# Parse and modify
import re
nums = re.findall(r'\d+', expr)
ops = re.findall(r'[+\-*/]', expr)
```

## Common Algorithms

### Binary Search

```python
def binary_search(arr, target):
    left, right = 0, len(arr) - 1
    while left <= right:
        mid = (left + right) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1
```

### BFS/DFS

```python
from collections import deque

# BFS
def bfs(graph, start):
    visited = set([start])
    queue = deque([start])
    while queue:
        node = queue.popleft()
        for neighbor in graph[node]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)

# DFS
def dfs(graph, node, visited=None):
    if visited is None:
        visited = set()
    visited.add(node)
    for neighbor in graph[node]:
        if neighbor not in visited:
            dfs(graph, neighbor, visited)
```

### Shortest Path (Dijkstra)

```python
import heapq

def dijkstra(graph, start):
    dist = {node: float('inf') for node in graph}
    dist[start] = 0
    pq = [(0, start)]
    
    while pq:
        d, u = heapq.heappop(pq)
        if d > dist[u]:
            continue
        for v, w in graph[u]:
            if dist[u] + w < dist[v]:
                dist[v] = dist[u] + w
                heapq.heappush(pq, (dist[v], v))
    return dist
```

## Data Structures

```python
from collections import defaultdict, Counter, deque
from heapq import heappush, heappop

# Default dict
graph = defaultdict(list)

# Counter
freq = Counter("aabbcc")  # {'a': 2, 'b': 2, 'c': 2}

# Priority queue
pq = []
heappush(pq, (priority, item))
item = heappop(pq)

# Deque (double-ended queue)
dq = deque([1, 2, 3])
dq.appendleft(0)
dq.pop()
```

## String Operations

```python
# Reverse
s[::-1]

# Check palindrome
s == s[::-1]

# Character frequency
from collections import Counter
Counter(s)

# ASCII conversions
ord('A')  # 65
chr(65)   # 'A'
```

## Math Operations

```python
import math

math.gcd(a, b)           # GCD
math.lcm(a, b)           # LCM (Python 3.9+)
math.factorial(n)        # n!
math.isqrt(n)            # Integer square root
pow(base, exp, mod)      # Modular exponentiation

# Check prime
def is_prime(n):
    if n < 2: return False
    for i in range(2, int(n**0.5) + 1):
        if n % i == 0:
            return False
    return True
```

## Time-Constrained Tips

```python
# Memoization
from functools import lru_cache

@lru_cache(maxsize=None)
def fib(n):
    if n < 2: return n
    return fib(n-1) + fib(n-2)

# Fast input for multiple lines
import sys
data = sys.stdin.read().split('\n')
```

## Useful One-Liners

```python
# Sum of digits
sum(int(d) for d in str(n))

# Flatten 2D list
flat = [x for row in matrix for x in row]

# Transpose matrix
transposed = list(zip(*matrix))

# All permutations
from itertools import permutations
list(permutations([1,2,3]))

# All combinations
from itertools import combinations
list(combinations([1,2,3], 2))
```
