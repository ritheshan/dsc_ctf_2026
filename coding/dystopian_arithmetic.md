# Dystopian Arithmetic

**Category:** Coding / Misc

---

## Challenge Description

> In this society, truth is declared.
> 
> 2 + 2 = 5.
> 
> Can you think the right way?

**Connection:**
```
nc math.challenges1.ctf.dscjssstuniv.in 8018
```

**Hints:**
1. Every answer is very close to the real one.
2. The difference is always exactly 1.
3. The transformation depends only on the real result.

---

## Analysis

### Initial Reconnaissance

The challenge involves a remote server that forces the user to solve math problems based on "Declared Truth" rather than objective math. The primary hint is the classic Orwellian prompt: **$2 + 2 = 5$**.

**Constraints:**
- **Questions:** 5 total
- **Time Limit:** 7 seconds per question
- **Logic:** The answer is always exactly 1 away from the real result

### Pattern Discovery

By interacting with the server, we can compare the objective results with the accepted "Dystopian" answers:

| Problem | Objective Result | Accepted Answer | Shift |
|---------|------------------|-----------------|-------|
| $5 - 8$ | $-3$ | $-2$ | $+1$ |
| $9 + 4$ | $13$ | $12$ | $-1$ |
| $10 + 6$ | $16$ | $15$ | $-1$ |
| $7 * 3$ | $21$ | $20$ | $-1$ |
| $7 - 5$ | $2$ | $1$ | $-1$ |

The logic confirms the hint: **"The difference is always exactly 1."** The transformation is tied to the result's value relative to the "Declared Truth" ($2+2=5$).

---

## Solution

### Exploitation Steps

1. **Handshake:** Respond `yes` to accepting the declared truth
2. **Calculation:** Compute the standard result of the arithmetic expression
3. **Adjustment:** Subtract 1 from the result (or add 1 for negative results)
4. **Submission:** Enter the adjusted value before the 7-second timeout

### Execution

```bash
nc math.challenges1.ctf.dscjssstuniv.in 8018
```

The challenge was solved manually by connecting via `nc` and performing rapid calculations with the $\pm1$ offset adjustment.

---

## Key Takeaways

- The challenge tests the ability to quickly identify a shifted logic pattern
- Apply the pattern under a time constraint
- By consistently applying a $\pm1$ offset, the user "aligns with the truth"

