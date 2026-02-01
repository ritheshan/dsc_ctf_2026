# Pathfinding Puzzle

**Category:** Coding / Misc

---

## Challenge Description

Navigate a 10x10 maze from Start (**S**) to End (**E**) while collecting special characters to form the hidden flag.

**Connection:**
```
nc pathfinding.challenges1.ctf.dscjssstuniv.in 8004
```

---

## Analysis

### Initial Reconnaissance

The challenge provides a netcat service that outputs a maze grid with the following constraints:

- **Walls (`#`):** Impassable tiles
- **Paths (`.`):** Empty tiles  
- **Start/End:** Navigate from `S` to `E` in the fewest steps
- **Collection:** Every character tile passed over is added to the "Collected" string

### Maze Layout

Based on the terminal output, the maze was structured as follows:

| Row | Content (Columns 0-9) |
|-----|----------------------|
| **0** | `S` `D` `S` `C` `C` `T` `F` `{` `P` `4` |
| **1** | `#` `#` `#` `#` `#` `#` `#` `#` `#` `T` |
| **2** | `#` `.` `.` `.` `.` `.` `.` `.` `#` `H` |
| **3** | `#` `.` `#` `#` `#` `#` `#` `.` `#` `}` |
| **4** | `#` `.` `#` `.` `.` `.` `#` `.` `.` `E` |

### Strategy

The "shortest path" follows the perimeter of the grid because the center is heavily obstructed by walls. To collect the maximum number of flag pieces, the path must traverse the entire top row before descending the final column.

**Path Coordinates:**
1. **Horizontal Phase:** `[0,0]` through `[0,9]`
2. **Vertical Phase:** `[1,9]` through `[4,9]`

---

## Solution

### Solve Script

```python
from pwn import *
import json

def solve():
    # Connection details
    host = 'pathfinding.challenges1.ctf.dscjssstuniv.in'
    port = 8004
    
    # Start connection
    conn = remote(host, port)
    
    # Receive initial maze data
    conn.recvuntil(b"Enter path (or 'q'):")
    
    # Manually defined shortest path to collect characters
    path = [
        [0,0],[0,1],[0,2],[0,3],[0,4],[0,5],[0,6],[0,7],[0,8],[0,9],
        [1,9],[2,9],[3,9],[4,9]
    ]

    # Convert to JSON and strip spaces as per server requirements
    payload = json.dumps(path).replace(" ", "")
    
    # Send solution
    conn.sendline(payload.encode())
    
    # Retrieve flag output
    print(conn.recvall().decode())

if __name__ == "__main__":
    solve()
```

### Execution

```bash
python solve.py
```

---

## Key Takeaways

- Understanding the maze structure is crucial for optimal pathfinding
- The flag characters are embedded in the path tiles
- JSON formatting must match server expectations (no spaces)