# Challenge: Bytes and Big Integers

## 📊 Challenge Info

| Property | Details |
|----------|---------|
| **Platform** | CryptoHack |
| **Track** | Introduction |
| **Category** | Encoding |
| **Difficulty** | 🟢 Easy |
| **Points** | 10 |
| **Author** | andreralf25 |

**Skills:** Big-integer encoding, `long_to_bytes()`, PyCryptodome, number theory basics

---

## 🎯 Challenge Description

> "Cryptographic operations like RSA work on integers, not strings. We need a standard way to convert between the two."
>
> Convert the following large integer back into a message:
>
> ```
> 11515195063862318899931685488813747395775516287369716132655853820958000
> ```

---

## 🧠 Concept Deep Dive

### Bytes as Integers

A block of bytes can be interpreted as a single big-endian integer. For example:

```
b'\x01\x02\x03'  ─►  1 * 256² + 2 * 256¹ + 3 * 256⁰  =  66051
```

In real cryptography (especially RSA), the message is padded and then converted to an integer before the modular exponentiation step. Reversing that conversion is essential for reading the result.

### `long_to_bytes()` vs Manual Conversion

Python's built-in `int.to_bytes()` works, but PyCryptodome's `Crypto.Util.number.long_to_bytes()` is the standard tool used across CTF scripts because it handles arbitrary-length integers automatically.

| Method | Example |
|--------|---------|
| `int.to_bytes(n, length, 'big')` | Requires knowing the byte length in advance |
| `long_to_bytes(n)` | Infers the length automatically |

### Visual Flow

```
big integer  ──► long_to_bytes() ──► raw bytes ──► .decode('ascii') ──► plaintext string
```

---

## 🔍 Solution Approach

### Key Insight

The large integer is simply the message bytes interpreted as a big-endian number. `long_to_bytes()` reverses this, giving back the original byte sequence.

### Step-by-Step Breakdown

1. **Import** `long_to_bytes` from `Crypto.Util.number`.
2. **Call** `long_to_bytes(n)` with the provided integer.
3. **Decode** the resulting bytes to an ASCII string to read the flag.

---

## 💻 Solution Code

```python
# Bytes and Big Integers — CryptoHack
# Convert a large integer back to the message it encodes

from Crypto.Util.number import long_to_bytes

n = 11515195063862318899931685488813747395775516287369716132655853820958000

# Reverse the big-endian byte encoding
message = long_to_bytes(n)

print(f"Flag: {message.decode('ascii')}")
```

**Output:**
```
Flag: crypto{3nc0d1n6_4ll_7h3_w4y_d0wn}
```

---

## 🚫 Common Pitfalls

- **`bytes_to_long` vs `long_to_bytes`**: Easy to mix up the direction. `bytes_to_long` encodes bytes → int; `long_to_bytes` decodes int → bytes.
  - *Fix*: Remember: `long_to_bytes` gives you back the **bytes** (the message).

- **Import path**: `long_to_bytes` lives in `Crypto.Util.number` (PyCryptodome), not the standard library.
  - *Fix*: Install with `pip install pycryptodome` and import as shown above.

- **Leading zero bytes**: If the original message started with a null byte (`\x00`), `long_to_bytes` will strip it. For CTF flags this is rarely an issue, but it matters in RSA padding.
  - *Fix*: Supply an explicit `blocksize` argument: `long_to_bytes(n, blocksize)`.

---

## 🧪 Practice Variations

1. Reverse the operation: convert the string `"hello"` to an integer using `bytes_to_long`.
2. What integer does the single byte `0xFF` represent? Verify with `bytes_to_long(b'\xff')`.
3. CryptoHack follow-up: RSA challenges in the Public-Key Cryptography track use this conversion for every decryption.

---

## 🌍 Real-World Applications

- **RSA encryption**: Plaintext is converted to an integer with `bytes_to_long`, encrypted as `c = m^e mod n`, then decrypted back to bytes with `long_to_bytes`.
- **Elliptic-curve signatures**: Message hashes are treated as large integers for the signing and verification math.
- **Protocol serialisation**: Many cryptographic protocols transmit public keys and signatures as big integers in binary (DER/ASN.1) format.

---

## 🔗 Resources

- [CryptoHack — Introduction Track](https://cryptohack.org/courses/intro/)
- [PyCryptodome `long_to_bytes` docs](https://pycryptodome.readthedocs.io/en/latest/src/util/util.html)
- [TOOLS.md](TOOLS.md)

---

## ✅ Flag

```
crypto{3nc0d1n6_4ll_7h3_w4y_d0wn}
```

---

[← Previous Challenge](Base64.md) | [🏠 Home](../README.md) | [Next Challenge →](XOR_Starter.md)

*CryptoHack CTF Writeup — andreralf25 — 2026*
