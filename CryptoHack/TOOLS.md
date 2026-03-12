# 🛠️ Tools Reference

> Python libraries, online tools, and quick-reference snippets used across CryptoHack and CTF challenges.

[🏠 Home](../README.md) | [🔓 CryptoHack](README.md)

---

## 📋 Table of Contents

1. [Python Libraries](#python-libraries)
2. [Online Tools](#online-tools)
3. [Quick Reference Snippets](#quick-reference-snippets)
4. [Debugging Tips](#debugging-tips)
5. [Installation](#installation)

---

## Python Libraries

### Standard Library (no install needed)

| Module | Key Functions | Use Case |
|--------|--------------|----------|
| `base64` | `b64encode`, `b64decode` | Base64 encoding/decoding |
| `binascii` | `hexlify`, `unhexlify` | Hex ↔ bytes conversion |
| `hashlib` | `md5`, `sha256`, `sha512` | Hashing |
| `itertools` | `cycle`, `product` | Key cycling, brute force |
| `math` | `gcd`, `isqrt`, `log` | Math utilities |
| `socket` | `socket`, `connect` | Network challenges |
| `struct` | `pack`, `unpack` | Binary data parsing |
| `string` | `ascii_letters`, `digits` | Character sets for brute force |

### PyCryptodome

The go-to library for cryptographic primitives in CTFs.

```bash
pip install pycryptodome
```

| Module | Key Functions | Use Case |
|--------|--------------|----------|
| `Crypto.Util.number` | `bytes_to_long`, `long_to_bytes`, `getPrime`, `inverse` | RSA math |
| `Crypto.Cipher.AES` | `new`, `encrypt`, `decrypt` | AES encryption |
| `Crypto.PublicKey.RSA` | `generate`, `import_key` | RSA key generation |
| `Crypto.Hash` | `SHA256`, `MD5` | Hashing |

### pwntools

Essential for network-based challenges and binary exploitation.

```bash
pip install pwntools
```

| Function | Use Case |
|----------|----------|
| `remote(host, port)` | Connect to a CTF server |
| `process(binary)` | Run a local binary |
| `p.sendline(data)` | Send data + newline |
| `p.recvline()` | Receive one line |
| `p.recvuntil(b":")` | Receive until a delimiter |
| `xor(a, b)` | XOR two values (handles different lengths) |

### SymPy

Useful for number theory and symbolic math.

```bash
pip install sympy
```

| Function | Use Case |
|----------|----------|
| `isprime(n)` | Primality test |
| `factorint(n)` | Integer factorization |
| `mod_inverse(a, n)` | Modular inverse |
| `discrete_log(p, a, g)` | Discrete logarithm |

---

## Online Tools

| Tool | URL | Best For |
|------|-----|----------|
| **CyberChef** | [gchq.github.io/CyberChef](https://gchq.github.io/CyberChef/) | Encoding/decoding, quick transforms |
| **factordb** | [factordb.com](http://factordb.com) | Factoring large integers for RSA |
| **dcode.fr** | [dcode.fr](https://www.dcode.fr/en) | Classic cipher encyclopedia |
| **CryptoHack** | [cryptohack.org](https://cryptohack.org) | Learning platform |
| **RsaCtfTool** | [GitHub](https://github.com/RsaCtfTool/RsaCtfTool) | Automated RSA attacks |

---

## Quick Reference Snippets

### Connect to a CTF Server

```python
# Using pwntools (recommended)
from pwn import remote

r = remote("challenge.server.com", 1337)
data = r.recvline().decode()
r.sendline(b"my_answer")
response = r.recvline().decode()
r.close()

# Using raw sockets
import socket
s = socket.socket()
s.connect(("challenge.server.com", 1337))
data = s.recv(4096).decode()
s.send(b"my_answer\n")
```

### XOR Operations

```python
# XOR two equal-length byte strings
def xor(a: bytes, b: bytes) -> bytes:
    return bytes(x ^ y for x, y in zip(a, b))

# XOR with a repeating key
def xor_key(data: bytes, key: bytes) -> bytes:
    return bytes(b ^ key[i % len(key)] for i, b in enumerate(data))

# Known-plaintext key recovery
def recover_key(ciphertext: bytes, known_plaintext: bytes) -> bytes:
    return bytes(c ^ p for c, p in zip(ciphertext, known_plaintext))
```

### RSA Operations

```python
from Crypto.Util.number import inverse, long_to_bytes

# Standard RSA decrypt when you have p, q
def rsa_decrypt(c, e, p, q):
    phi = (p - 1) * (q - 1)
    d = inverse(e, phi)
    m = pow(c, d, p * q)
    return long_to_bytes(m)

# Factor N when N is even (one prime is 2)
def factor_even_n(N, e, c):
    p, q = 2, N // 2
    return rsa_decrypt(c, e, p, q)
```

### ROT13 / Caesar Cipher

```python
import codecs

# ROT13
decoded = codecs.decode("cvpbPGS{test}", "rot_13")

# General Caesar cipher
def caesar(text: str, shift: int) -> str:
    result = []
    for ch in text:
        if ch.isalpha():
            base = ord('A') if ch.isupper() else ord('a')
            result.append(chr((ord(ch) - base + shift) % 26 + base))
        else:
            result.append(ch)
    return ''.join(result)

# Brute force all 25 shifts
for shift in range(1, 26):
    print(f"Shift {shift}: {caesar(ciphertext, shift)}")
```

### Brute Force Skeleton

```python
import string

CHARSET = string.ascii_letters + string.digits  # 62 characters

for char in CHARSET:
    candidate = known_prefix + char
    result = try_key(candidate)
    if is_valid(result):
        print(f"Found: {candidate}")
        break
```

---

## Debugging Tips

### 1. Always print intermediate values

```python
ciphertext = bytes.fromhex(hex_string)
print(f"[*] Ciphertext length: {len(ciphertext)}")
print(f"[*] First 8 bytes: {ciphertext[:8].hex()}")
```

### 2. Check your byte/string types

```python
# Common mistake: mixing str and bytes
key = "secret"         # str — won't XOR with bytes
key = b"secret"        # bytes — correct

# Convert if needed
key = "secret".encode()
```

### 3. Handle encoding errors gracefully

```python
try:
    result = plaintext.decode('utf-8')
except UnicodeDecodeError:
    result = plaintext.decode('latin-1')  # Fallback
    print("[!] Warning: non-UTF-8 output")
```

### 4. Verify your flag format

```python
flag = plaintext.decode()
assert flag.startswith("crypto{") and flag.endswith("}"), \
    f"Unexpected output: {flag!r}"
print(f"[+] Flag: {flag}")
```

---

## Installation

```bash
# Install all common CTF dependencies at once
pip install pycryptodome pwntools sympy requests

# Verify
python3 -c "from Crypto.Util.number import getPrime; print(getPrime(64))"
```

---

[🏠 Home](../README.md) | [🔓 CryptoHack](README.md) | [📖 Concepts](CONCEPTS.md) | [🧪 Practice](PRACTICE.md)
