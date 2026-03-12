# 📖 Cryptography Concepts Reference

> Quick reference for the core concepts that appear throughout CryptoHack challenges.

[🏠 Home](../README.md) | [🔓 CryptoHack](README.md)

---

## 📋 Table of Contents

1. [XOR Properties](#xor-properties)
2. [Encoding Methods](#encoding-methods)
3. [Number Base Conversions](#number-base-conversions)
4. [ASCII & Unicode Reference](#ascii--unicode-reference)
5. [Common Python Cryptography Functions](#common-python-cryptography-functions)
6. [Modular Arithmetic Basics](#modular-arithmetic-basics)

---

## XOR Properties

XOR (exclusive-or, `⊕`) is the backbone of many cryptographic operations. Understanding its properties is essential.

### Truth Table

| A | B | A ⊕ B |
|---|---|-------|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

### Key Properties

| Property | Formula | Meaning |
|----------|---------|---------|
| Commutativity | `A ⊕ B = B ⊕ A` | Order doesn't matter |
| Associativity | `(A ⊕ B) ⊕ C = A ⊕ (B ⊕ C)` | Grouping doesn't matter |
| Identity | `A ⊕ 0 = A` | XOR with zero does nothing |
| Self-inverse | `A ⊕ A = 0` | XOR with itself gives zero |
| Key cancellation | `(A ⊕ K) ⊕ K = A` | Applying the same key twice recovers plaintext |

### Known-Plaintext Attack

Because `ciphertext ⊕ plaintext = key`, knowing any portion of the plaintext lets you directly compute the corresponding key bytes:

```
key[i] = ciphertext[i] ⊕ known_plaintext[i]
```

This is why XOR with a **short repeating key** is insecure whenever any plaintext is known (e.g., a predictable flag prefix like `crypto{` or `THM{`).

---

## Encoding Methods

Encoding is not encryption — it's a reversible transformation with no secret key.

### Comparison Table

| Method | Input | Output | Use Case | Python |
|--------|-------|--------|----------|--------|
| ASCII | Characters | 7-bit integers (0–127) | Text ↔ numbers | `ord()`, `chr()` |
| Hex | Bytes | 0–9, a–f characters | Binary data as readable text | `bytes.hex()`, `bytes.fromhex()` |
| Base64 | Bytes | A–Z, a–z, 0–9, +, / | Binary data in text contexts (email, URLs) | `base64.b64encode/decode()` |
| Base10 (int) | Bytes | Decimal integer | RSA message preprocessing | `int.from_bytes()`, `.to_bytes()` |

### Why Base64 Exists

Base64 converts 3 bytes (24 bits) into 4 printable characters (6 bits each). It was invented for email systems that could only handle 7-bit ASCII — not arbitrary binary data. The `=` padding characters appear when the input length isn't a multiple of 3.

```
Input bytes:  [0x4D, 0x61, 0x6E]   = "Man"
Binary:        01001101 01100001 01101110
6-bit groups:  010011  010110  000101  101110
Base64 chars:  T       W       F       u     → "TWFu"
```

---

## Number Base Conversions

### Visual Flow

```
Binary  ──► Hex ──► Decimal ──► Base64
  │                                │
  └──────────── ASCII ◄────────────┘
```

### Conversion Table

| From\To | Binary | Hex | Decimal | ASCII |
|---------|--------|-----|---------|-------|
| Binary | — | Group 4 bits | `int(b, 2)` | `chr(int(b,2))` |
| Hex | `bin(int(h,16))` | — | `int(h, 16)` | `bytes.fromhex(h)` |
| Decimal | `bin(n)` | `hex(n)` | — | `chr(n)` |
| ASCII | `bin(ord(c))` | `hex(ord(c))` | `ord(c)` | — |

### Quick Examples

```python
# Decimal → Hex
hex(255)         # '0xff'

# Hex → Bytes → String
bytes.fromhex("48656c6c6f")  # b'Hello'

# String → Hex
"Hello".encode().hex()       # '48656c6c6f'

# Bytes → Big Integer
int.from_bytes(b"Hello", 'big')   # 310939249775

# Big Integer → Bytes
(310939249775).to_bytes(5, 'big') # b'Hello'
```

---

## ASCII & Unicode Reference

### Printable ASCII Ranges

| Range | Description | Example |
|-------|-------------|---------|
| 32–47 | Special chars | space, !, ", #, $, %, &, ', (, ), *, +, comma, -, ., / |
| 48–57 | Digits | 0–9 |
| 58–64 | Special chars | :, ;, <, =, >, ?, @ |
| 65–90 | Uppercase letters | A–Z |
| 91–96 | Special chars | [, \\, ], ^, _, ` |
| 97–122 | Lowercase letters | a–z |
| 123–126 | Special chars | {, \|, }, ~ |

### ASCII vs Unicode

| Feature | ASCII | Unicode (UTF-8) |
|---------|-------|-----------------|
| Size | 7 bits (128 chars) | Variable (1–4 bytes) |
| Covers | English alphabet + symbols | All world languages |
| Python | `ord()` / `chr()` | `encode('utf-8')` |
| Backwards compatible | — | Yes — ASCII is a subset of UTF-8 |

---

## Common Python Cryptography Functions

### Built-in

```python
# XOR two byte strings of equal length
xored = bytes(a ^ b for a, b in zip(bytes1, bytes2))

# XOR with repeating key
key = b"secret"
xored = bytes(b ^ key[i % len(key)] for i, b in enumerate(plaintext))

# Convert hex string to bytes
data = bytes.fromhex("deadbeef")

# Convert bytes to hex string
hexstr = data.hex()

# Convert bytes to integer (big-endian)
n = int.from_bytes(b"hello", 'big')

# Convert integer to bytes
b = n.to_bytes((n.bit_length() + 7) // 8, 'big')
```

### Base64

```python
import base64

encoded = base64.b64encode(b"Hello, World!")   # b'SGVsbG8sIFdvcmxkIQ=='
decoded = base64.b64decode(b'SGVsbG8sIFdvcmxkIQ==')  # b'Hello, World!'
```

### PyCryptodome

```python
from Crypto.Util.number import bytes_to_long, long_to_bytes, inverse, getPrime

# Convert message to integer for RSA
m = bytes_to_long(b"secret message")

# Convert back
msg = long_to_bytes(m)

# Modular inverse (for RSA private key)
d = inverse(e, phi)

# Generate a random prime
p = getPrime(512)  # 512-bit prime
```

---

## Modular Arithmetic Basics

### Key Concepts

| Concept | Formula | Notes |
|---------|---------|-------|
| Modulo | `a mod n` | Remainder when dividing `a` by `n` |
| Modular inverse | `a⁻¹ mod n` | The value `x` such that `a·x ≡ 1 (mod n)` |
| Fermat's little theorem | `aᵖ⁻¹ ≡ 1 (mod p)` | Holds when `p` is prime and `gcd(a,p)=1` |
| Euler's theorem | `aᵠ⁽ⁿ⁾ ≡ 1 (mod n)` | Generalisation of Fermat's |
| Euler's totient φ(n) | `(p-1)(q-1)` | For `n = p*q` with p, q prime |

### Python Examples

```python
# Modulo
17 % 5          # 2

# Modular exponentiation (fast)
pow(base, exp, mod)   # Never compute base**exp first!

# Modular inverse (Python 3.8+)
pow(a, -1, n)

# Or with math.gcd for older versions
from math import gcd
def mod_inverse(a, n):
    if gcd(a, n) != 1:
        raise ValueError("Inverse doesn't exist")
    return pow(a, -1, n)
```

---

[🏠 Home](../README.md) | [🔓 CryptoHack](README.md) | [🛠️ Tools](TOOLS.md)
