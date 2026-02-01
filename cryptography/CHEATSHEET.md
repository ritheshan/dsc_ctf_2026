# Cryptography Cheatsheet

## Identification

```bash
# Identify hash type
hashid <hash>
hash-identifier

# Identify encoding
# Base64: ends with = or ==, uses A-Za-z0-9+/
# Hex: only 0-9a-f
# Base32: A-Z2-7, padding with =
```

## Encoding/Decoding

### Base64

```bash
echo "text" | base64
echo "dGV4dA==" | base64 -d
```

```python
import base64
base64.b64encode(b"text")
base64.b64decode(b"dGV4dA==")
```

### Hex

```bash
echo "text" | xxd -p
echo "74657874" | xxd -r -p
```

```python
bytes.fromhex("74657874")
"text".encode().hex()
```

### Binary

```python
# Text to binary
''.join(format(ord(c), '08b') for c in "text")

# Binary to text
''.join(chr(int(b, 2)) for b in binary.split())
```

## Classic Ciphers

### Caesar Cipher

```python
def caesar_decrypt(text, shift):
    result = ""
    for char in text:
        if char.isalpha():
            base = ord('A') if char.isupper() else ord('a')
            result += chr((ord(char) - base - shift) % 26 + base)
        else:
            result += char
    return result

# Brute force all shifts
for i in range(26):
    print(f"{i}: {caesar_decrypt(ciphertext, i)}")
```

### ROT13

```python
import codecs
codecs.decode("grkg", 'rot_13')  # "text"
```

### Vigenere

```python
def vigenere_decrypt(ciphertext, key):
    result = ""
    key = key.upper()
    key_idx = 0
    for char in ciphertext:
        if char.isalpha():
            shift = ord(key[key_idx % len(key)]) - ord('A')
            base = ord('A') if char.isupper() else ord('a')
            result += chr((ord(char) - base - shift) % 26 + base)
            key_idx += 1
        else:
            result += char
    return result
```

### XOR

```python
def xor_decrypt(data, key):
    return bytes([b ^ key[i % len(key)] for i, b in enumerate(data)])

# Single byte XOR brute force
for key in range(256):
    result = bytes([b ^ key for b in ciphertext])
    if b'flag' in result.lower():
        print(key, result)
```

## RSA

### Basic RSA Math

```python
from Crypto.Util.number import inverse, long_to_bytes, bytes_to_long

# Given: n, e, c
# Find: p, q such that n = p * q
# Calculate: phi = (p-1)(q-1)
# Calculate: d = inverse(e, phi)
# Decrypt: m = pow(c, d, n)

n = ...
e = 65537
c = ...
p = ...
q = ...

phi = (p - 1) * (q - 1)
d = inverse(e, phi)
m = pow(c, d, n)
print(long_to_bytes(m))
```

### RSA Attacks

```bash
# Factor n (if small)
factordb.com
yafu

# Common modulus attack
# Wiener attack (small d)
# Hastad broadcast attack (small e, same m)
```

```python
# RsaCtfTool
python3 RsaCtfTool.py -n <n> -e <e> --uncipher <c>
```

## Hash Cracking

```bash
# John the Ripper
john --wordlist=rockyou.txt hash.txt
john --show hash.txt

# Hashcat
hashcat -m 0 hash.txt rockyou.txt     # MD5
hashcat -m 100 hash.txt rockyou.txt   # SHA1
hashcat -m 1400 hash.txt rockyou.txt  # SHA256

# Online
crackstation.net
hashes.com
```

## Tools

```bash
# CyberChef (online)
gchq.github.io/CyberChef/

# dcode.fr (cipher identifier and solver)
dcode.fr

# quipqiup (substitution cipher solver)
quipqiup.com

# RsaCtfTool
github.com/Ganapati/RsaCtfTool
```

## Python Crypto Libraries

```python
from Crypto.Cipher import AES, DES
from Crypto.Util.number import inverse, GCD, bytes_to_long, long_to_bytes
from Crypto.Util.Padding import pad, unpad
import hashlib

# Hashing
hashlib.md5(b"text").hexdigest()
hashlib.sha256(b"text").hexdigest()

# AES
cipher = AES.new(key, AES.MODE_ECB)
plaintext = cipher.decrypt(ciphertext)
```

## Common Patterns

| Pattern | Likely Cipher |
|---------|---------------|
| `==` at end | Base64 |
| Only hex chars | Hex encoded |
| 32 hex chars | MD5 hash |
| 40 hex chars | SHA1 hash |
| 64 hex chars | SHA256 hash |
| Letter substitution | Caesar, ROT13, Vigenere |
| Repeating pattern | XOR with short key |
