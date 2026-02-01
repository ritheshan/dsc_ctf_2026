# Reverse Engineering Cheatsheet

## Static Analysis

### File Information

```bash
file binary
strings binary
strings -n 8 binary       # Min 8 chars
strings -e l binary       # 16-bit little endian

readelf -h binary         # ELF header
readelf -s binary         # Symbols
readelf -a binary         # All info

objdump -d binary         # Disassemble
objdump -t binary         # Symbol table
```

### Decompilers

```bash
# Ghidra (free)
ghidraRun

# IDA Free
# Binary Ninja
# Hopper (macOS)

# Online
dogbolt.org              # Multiple decompilers
```

### Common Patterns

```c
// Flag check pattern
if (strcmp(input, "secret") == 0) {
    print_flag();
}

// XOR encryption
for (int i = 0; i < len; i++) {
    data[i] ^= key;
}

// Custom encoding
encoded[i] = (input[i] + offset) % 256;
```

## Dynamic Analysis

### GDB Commands

```bash
gdb ./binary
gdb -q ./binary          # Quiet mode

# Breakpoints
b main
b *0x08048456
b *main+50

# Run
r
r arg1 arg2
r < input.txt

# Step
n                        # Next (over)
s                        # Step (into)
ni                       # Next instruction
si                       # Step instruction
c                        # Continue

# Examine
x/10x $rsp               # 10 hex words
x/s 0x08048500           # String
x/i $rip                 # Instruction
info registers
p $rax
p/x $rax                 # Hex format
```

### GDB with Pwndbg/GEF

```bash
# Enhanced commands
checksec
vmmap
heap
got
plt
```

### Ltrace/Strace

```bash
ltrace ./binary          # Library calls
strace ./binary          # System calls
ltrace -s 100 ./binary   # Longer strings
```

## Common Challenges

### Password Check Bypass

```python
# Patch binary to bypass check
# Change JNE to JE or NOP

# GDB: Set register
set $eax = 1
set $rip = 0x08048500
```

### Anti-Debugging

```c
// Common checks
ptrace(PTRACE_TRACEME, 0, 0, 0)
isDebuggerPresent()
/proc/self/status (TracerPid)

// Bypass
# Patch the check
# LD_PRELOAD with fake function
# GDB: skip function
```

### Encrypted Strings

```python
# XOR decrypt
def xor_decrypt(data, key):
    return bytes([b ^ key for b in data])

# Find key by known plaintext
# If "flag" appears, try XOR with expected chars
```

## Assembly Quick Reference

### x86/x64 Registers

```
# 64-bit    32-bit    16-bit    8-bit
rax         eax       ax        al
rbx         ebx       bx        bl
rcx         ecx       cx        cl
rdx         edx       dx        dl
rsi         esi       si        sil
rdi         edi       di        dil
rbp         ebp       bp        bpl
rsp         esp       sp        spl
```

### Common Instructions

```asm
mov  dst, src      ; dst = src
add  dst, src      ; dst += src
sub  dst, src      ; dst -= src
xor  dst, src      ; dst ^= src
cmp  a, b          ; compare (sets flags)
test a, b          ; AND (sets flags)
jmp  addr          ; unconditional jump
je   addr          ; jump if equal
jne  addr          ; jump if not equal
call addr          ; call function
ret                ; return
push val           ; push to stack
pop  reg           ; pop from stack
```

### Calling Conventions

```
# x64 Linux: rdi, rsi, rdx, rcx, r8, r9
# x64 Windows: rcx, rdx, r8, r9
# x86: stack (right to left)
```

## Python Disassembly

```python
import dis
dis.dis(function)

# Bytecode analysis
import marshal
code = marshal.loads(pyc_content[16:])  # Skip header
```

## Java/Android

```bash
# Decompile JAR
jadx file.jar
jd-gui file.jar

# APK
apktool d app.apk
jadx app.apk
```

## .NET

```bash
# Decompile
dnSpy
ILSpy
dotPeek
```

## Tools Summary

| Tool | Purpose |
|------|---------|
| Ghidra | Free decompiler |
| IDA | Industry standard disassembler |
| GDB | Debugger |
| radare2 | RE framework |
| objdump | Quick disassembly |
| strings | Extract strings |
| ltrace/strace | Trace calls |
| jadx | Java/Android decompiler |
| dnSpy | .NET decompiler |

## Quick Workflow

1. `file` - Identify binary type
2. `strings` - Look for flags/hints
3. `checksec` - Check protections
4. Load in Ghidra/IDA - Find main/interesting functions
5. Identify key logic (comparisons, loops)
6. Dynamic analysis with GDB if needed
7. Write keygen/solution
