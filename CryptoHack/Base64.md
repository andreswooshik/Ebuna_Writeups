# Challenge: Base64

## 📊 Challenge Info

| Property | Details |
|----------|---------|
| **Platform** | CryptoHack |
| **Track** | Introduction |
| **Category** | Encoding |
| **Difficulty** | 🟢 Easy |
| **Points** | 10 |
| **Author** | andreralf25 |

**Skills:** Base64 encoding, hex-to-bytes conversion, Python `base64` module

---

## 🎯 Challenge Description

> "Base64 encoding is used to safely transport binary data over text-based channels."
>
> Take the following hex string, decode it to bytes, and then encode the result as Base64:
>
> ```
> 72bca9b68fc16ac7beeb8f849dca1d8a783e8acf9679bf9269f7bf
> ```

---

## 🧠 Concept Deep Dive

### What is Base64?

Base64 represents binary data using only 64 printable characters: `A–Z`, `a–z`, `0–9`, `+`, and `/`. Every 3 bytes of binary data become 4 Base64 characters, and `=` padding is added to make the output length a multiple of 4.

| Binary bytes | Base64 output |
|--------------|--------------|
| `0x48 0x65 0x6c` | `SGVs` |
| `0x6c 0x6f` | `bG8=` |

### Why Use Base64?

Many systems (email, HTML, JSON, URLs) can only handle printable text. Base64 is a universal way to embed arbitrary binary data — such as cryptographic keys, certificates, or encrypted payloads — in a text-safe format.

### Visual Flow

```
hex string  ──► bytes.fromhex() ──► raw bytes ──► base64.b64encode() ──► Base64 string
```

---

## 🔍 Solution Approach

### Key Insight

This is a two-step encoding conversion: hex → bytes → Base64. Python's standard library handles both steps without any third-party packages.

### Step-by-Step Breakdown

1. **Decode the hex string** to raw bytes using `bytes.fromhex()`.
2. **Encode the bytes** to Base64 using `base64.b64encode()`.
3. The result is a `bytes` object containing the Base64 string; `.decode()` converts it to a plain Python string.

---

## 💻 Solution Code

```python
# Base64 — CryptoHack
# Convert a hex-encoded string to its Base64 representation

import base64

hex_string = "72bca9b68fc16ac7beeb8f849dca1d8a783e8acf9679bf9269f7bf"

# Step 1: hex → raw bytes
raw_bytes = bytes.fromhex(hex_string)

# Step 2: raw bytes → Base64
flag = base64.b64encode(raw_bytes).decode('ascii')

print(f"Flag: {flag}")
```

**Output:**
```
Flag: crypto/Base+64+Encoding+is+Web+Safe/
```

---

## 🚫 Common Pitfalls

- **Confusing encoding with encryption**: Base64 is purely an encoding — it provides **no security**. Anyone who sees a Base64 string can decode it trivially.
  - *Fix*: Never use Base64 as a substitute for encryption.

- **URL-unsafe characters**: Standard Base64 uses `+` and `/`, which are special characters in URLs.
  - *Fix*: Use `base64.urlsafe_b64encode()` for URL-safe output, which replaces `+` with `-` and `/` with `_`.

- **Forgetting the padding `=`**: Some implementations strip trailing `=` padding. This can cause `binascii.Error: Incorrect padding` when decoding.
  - *Fix*: Re-add padding before decoding: `data += '=' * (-len(data) % 4)`.

---

## 🧪 Practice Variations

1. Decode a Base64 string back to bytes using `base64.b64decode()`.
2. Encode the string `"Hello, World!"` to Base64. What is the output?
3. What is the difference between `b64encode` and `urlsafe_b64encode`? When would you use each?

---

## 🌍 Real-World Applications

- **JWT (JSON Web Tokens)**: The header and payload are Base64url-encoded and transmitted in HTTP headers.
- **Data URIs**: Images embedded directly in HTML use Base64 encoding (`<img src="data:image/png;base64,...">`).
- **PEM certificates**: TLS certificates (`.pem` / `.crt` files) wrap Base64-encoded DER data between `-----BEGIN CERTIFICATE-----` markers.

---

## 🔗 Resources

- [CryptoHack — Introduction Track](https://cryptohack.org/courses/intro/)
- [Python `base64` module docs](https://docs.python.org/3/library/base64.html)
- [CONCEPTS.md](CONCEPTS.md)

---

## ✅ Flag

```
crypto/Base+64+Encoding+is+Web+Safe/
```

---

[← Previous Challenge](HEX.md) | [🏠 Home](../README.md) | [Next Challenge →](Bytes_and_Big_Integers.md)

*CryptoHack CTF Writeup — andreralf25 — 2026*
