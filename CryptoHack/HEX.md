# Challenge: HEX

## 📊 Challenge Info

| Property | Details |
|----------|---------|
| **Platform** | CryptoHack |
| **Track** | Introduction |
| **Category** | Encoding |
| **Difficulty** | 🟢 Easy |
| **Points** | 5 |
| **Author** | andreralf25 |

**Skills:** Hexadecimal encoding, Python `bytes.fromhex()`, base-16 number system

---

## 🎯 Challenge Description

> "When we work with cryptographic operations, we often need a way to represent raw bytes as text. Hexadecimal (base 16) is one of the most common formats."
>
> Decode the following hex string to get the flag:
>
> ```
> 63727970746f7b70343535773072645f31355f68337d
> ```

---

## 🧠 Concept Deep Dive

### What is Hexadecimal?

Hexadecimal is base-16. Each hex digit represents 4 bits, so every **byte** (8 bits) is written as exactly **two hex digits**:

| Decimal | Binary | Hex |
|---------|--------|-----|
| 0 | 0000 0000 | 00 |
| 15 | 0000 1111 | 0f |
| 99 | 0110 0011 | 63 |
| 255 | 1111 1111 | ff |

### Why Hex is Useful in Cryptography

- **Compact**: two characters represent one byte, making long byte strings readable.
- **Unambiguous**: no confusion between text characters and raw bytes.
- **Universal**: used in almost every crypto library, debugger, and hex editor.

### Visual Flow

```
hex string:  "63 72 79 70 74 6f 7b ..."
                  ↓  bytes.fromhex()
bytes object: b'\x63\x72\x79\x70\x74\x6f\x7b ...'
                  ↓  .decode('ascii')
string:       "crypto{ ..."
```

---

## 🔍 Solution Approach

### Key Insight

Python's `bytes.fromhex()` converts a hex string directly to a bytes object. Decoding that bytes object with `.decode('ascii')` (or `.decode()`) gives the plaintext flag.

### Step-by-Step Breakdown

1. **Take the hex string** from the challenge.
2. **Convert to bytes** using `bytes.fromhex(hex_string)`.
3. **Decode to text** using `.decode('ascii')` — since all bytes are in the printable ASCII range.

---

## 💻 Solution Code

```python
# HEX — CryptoHack
# Decode a hex-encoded string back to plaintext

hex_string = "63727970746f7b70343535773072645f31355f68337d"

# bytes.fromhex() converts each pair of hex digits to a byte
flag = bytes.fromhex(hex_string).decode('ascii')

print(f"Flag: {flag}")
```

**Output:**
```
Flag: crypto{p455w0rd_15_h3x}
```

---

## 🚫 Common Pitfalls

- **Odd-length hex strings**: `bytes.fromhex()` requires an even number of hex characters (2 per byte).
  - *Fix*: Ensure the hex string has no spaces or extra characters, or pad with a leading `0` if necessary.

- **Using `int(hex_string, 16)`**: This converts the whole string to a single integer, not a byte sequence.
  - *Fix*: Use `bytes.fromhex()` when you want raw bytes, not a big integer.

- **Wrong encoding on decode**: Using `.decode('utf-8')` on ASCII-only data is fine, but non-ASCII bytes will raise a `UnicodeDecodeError`.
  - *Fix*: Use `.decode('latin-1')` as a fallback when bytes might be outside the ASCII range.

---

## 🧪 Practice Variations

1. Go the other way: encode the string `"hello"` to hex using `.hex()` on a bytes object.
2. What hex string represents the ASCII character `'A'`? Verify with Python.
3. CryptoHack follow-up: [Base64](Base64.md) — another common encoding for binary data.

---

## 🌍 Real-World Applications

- **TLS/SSL**: Certificate fingerprints and session keys are displayed in hex.
- **Hashing**: SHA-256 outputs (digests) are almost always shown as hex strings.
- **Malware analysis**: Shellcode and binary payloads are represented in hex within disassemblers.

---

## 🔗 Resources

- [CryptoHack — Introduction Track](https://cryptohack.org/courses/intro/)
- [Python `bytes.fromhex()` docs](https://docs.python.org/3/library/stdtypes.html#bytes.fromhex)
- [CONCEPTS.md](CONCEPTS.md)

---

## ✅ Flag

```
crypto{p455w0rd_15_h3x}
```

---

[← Previous Challenge](ASCII.md) | [🏠 Home](../README.md) | [Next Challenge →](Base64.md)

*CryptoHack CTF Writeup — andreralf25 — 2026*
