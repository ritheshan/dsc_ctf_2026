# Miscellaneous Challenges Cheatsheet

## Encoding Detection & Conversion

```bash
# CyberChef (best tool for encoding chains)
gchq.github.io/CyberChef/

# Common encodings
base64 -d
xxd -r -p          # Hex to binary
```

### Multi-layer Decoding

```python
import base64

data = "encoded_string"

# Try multiple decodings
while True:
    try:
        data = base64.b64decode(data).decode()
        print(data)
    except:
        break
```

## Esoteric Languages

### Brainfuck

```
++++++++++[>+++++++>++++++++++>+++>+<<<<-]
```

```bash
# Online interpreters
copy.sh/brainfuck
```

### Ook!

```
Ook. Ook! Ook. Ook?
```

### Whitespace

```
# Only spaces, tabs, newlines
# Use online interpreter
```

### Piet

```
# Visual programming language (images)
```

### JSFuck

```javascript
[][(![]+[])[+[]]+(![]+[])[!+[]+!+[]]
```

## QR Codes & Barcodes

```bash
# Read QR code
zbarimg qrcode.png

# Create QR code
qrencode -o output.png "text"

# Online
webqr.com
```

## Morse Code

```python
MORSE = {
    'A': '.-', 'B': '-...', 'C': '-.-.', 'D': '-..', 'E': '.',
    'F': '..-.', 'G': '--.', 'H': '....', 'I': '..', 'J': '.---',
    'K': '-.-', 'L': '.-..', 'M': '--', 'N': '-.', 'O': '---',
    'P': '.--.', 'Q': '--.-', 'R': '.-.', 'S': '...', 'T': '-',
    'U': '..-', 'V': '...-', 'W': '.--', 'X': '-..-', 'Y': '-.--',
    'Z': '--..', '0': '-----', '1': '.----', '2': '..---',
    '3': '...--', '4': '....-', '5': '.....', '6': '-....',
    '7': '--...', '8': '---..', '9': '----.'
}
MORSE_REV = {v: k for k, v in MORSE.items()}

def decode_morse(code):
    words = code.split('   ')
    return ' '.join(''.join(MORSE_REV.get(c, '?') for c in word.split()) for word in words)
```

## Number Systems

```python
# Binary to decimal
int('1010', 2)  # 10

# Decimal to binary
bin(10)  # '0b1010'

# Hex conversions
hex(255)         # '0xff'
int('ff', 16)    # 255

# Octal
oct(8)           # '0o10'
int('10', 8)     # 8

# ASCII
chr(65)          # 'A'
ord('A')         # 65
```

## Common Patterns

### Flag Formats

```
flag{...}
FLAG{...}
ctf{...}
CTF{...}
DSCCTF{...}
```

### Look For

```bash
# Grep for flag patterns
grep -rioE "(flag|ctf)\{[^}]+\}" .
strings file | grep -iE "(flag|ctf)"
```

## Trivia & References

```
# Check for:
- Movie/book references
- Famous quotes
- Historical events
- Pop culture
- Leet speak (1337)
```

## Netcat Automation

```python
from pwn import *

conn = remote('host', port)

# Receive until prompt
conn.recvuntil(b'>')

# Send response
conn.sendline(b'answer')

# Interactive mode
conn.interactive()
```

## File Manipulation

```bash
# Convert between formats
convert input.png output.jpg

# Split file
split -b 1000 bigfile part_

# Combine files
cat part_* > combined

# Compare files
diff file1 file2
cmp file1 file2
```

## Online Resources

| Resource | URL |
|----------|-----|
| CyberChef | gchq.github.io/CyberChef |
| dcode.fr | dcode.fr |
| Brainfuck | copy.sh/brainfuck |
| QR Decode | webqr.com |
