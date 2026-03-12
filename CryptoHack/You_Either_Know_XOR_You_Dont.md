# Challenge: You Either Know, XOR You Don't

## 📊 Challenge Info

| Property | Details |
|----------|---------|
| **Platform** | CryptoHack |
| **Track** | Introduction |
| **Category** | XOR / Known-Plaintext Attack |
| **Difficulty** | 🟢 Easy |
| **Points** | 30 |
| **Author** | andreralf25 |

**Skills:** XOR cipher, known-plaintext attack, repeating-key XOR, key recovery

---

## 🎯 Challenge Description

> "I've encrypted the flag with my secret key, you'll never be able to guess it."
>
> **Ciphertext (hex):** `0e0b213f26041e480b26217f27342e175d0e070a3c5b103e2526217f27342e175d0e077e263451150104`
>
> **Hint:** "Remember the flag format and how it might help you in this challenge!"

**Flag format:** `crypto{...}`

---

## 🧠 Concept Deep Dive

### What is XOR Encryption?

XOR (eXclusive OR) encryption works by combining each byte of the plaintext with a byte of the key using the XOR bitwise operation:

```
ciphertext[i] = plaintext[i] ⊕ key[i % key_length]
```

When the key is shorter than the message, it **repeats** — this is called a repeating-key XOR cipher.

### Why Repeating-Key XOR Is Insecure

XOR has a critical property: it is its own inverse.

```
Encrypt:  plaintext  ⊕ key = ciphertext
Decrypt:  ciphertext ⊕ key = plaintext
Recover:  ciphertext ⊕ plaintext = key   ← this is the vulnerability
```

If you know *any* part of the plaintext, you can directly compute the corresponding key bytes — no brute force needed. In CTFs, flags always follow a predictable format (`crypto{...}`), giving us the first 7 bytes of plaintext for free.

### Visual Flow

```
Known plaintext:   c  r  y  p  t  o  {
                   ↕  ↕  ↕  ↕  ↕  ↕  ↕  ← XOR each pair
Ciphertext:       0e 0b 21 3f 26 04 1e
                   ↓  ↓  ↓  ↓  ↓  ↓  ↓
Key fragment:     6d 79 58 4f 52 6b 65
                   m  y  X  O  R  k  e    → "myXORke" → "myXORkey"
```

---

## 🔍 Solution Approach

### Vulnerability

XOR with a short repeating key is trivially broken by a **known-plaintext attack** when any part of the plaintext is known. Here, the flag prefix `crypto{` (7 bytes) gives us 7 of the 8 key bytes instantly.

### Step-by-Step Breakdown

**Step 1 — Extract the known prefix bytes**

Every CryptoHack flag starts with `crypto{` (7 bytes):
```
ASCII:  c    r    y    p    t    o    {
Hex:   63   72   79   70   74   6f   7b
```

**Step 2 — Recover the key fragment**

XOR each ciphertext byte with the corresponding known plaintext byte:

| Byte | Ciphertext | Known Plaintext | Key Byte (hex) | Key Byte (ASCII) |
|------|------------|-----------------|----------------|------------------|
| 0 | `0e` | `63` (c) | `6d` | **m** |
| 1 | `0b` | `72` (r) | `79` | **y** |
| 2 | `21` | `79` (y) | `58` | **X** |
| 3 | `3f` | `70` (p) | `4f` | **O** |
| 4 | `26` | `74` (t) | `52` | **R** |
| 5 | `04` | `6f` (o) | `6b` | **k** |
| 6 | `1e` | `7b` ({) | `65` | **e** |

Key fragment: **`myXORke`**

**Step 3 — Deduce the full key**

The fragment `myXORke` is clearly the start of the English word **`myXORkey`** (8 characters). This is the full repeating key.

**Step 4 — Decrypt the full ciphertext**

Apply the 8-byte key cyclically across all 40 ciphertext bytes:

```
plaintext[i] = ciphertext[i] ⊕ key[i % 8]
```

---

## 💻 Solution Code

```python
# CryptoHack: You Either Know, XOR You Don't
# Known-plaintext attack to recover the repeating XOR key

ciphertext_hex = "0e0b213f26041e480b26217f27342e175d0e070a3c5b103e2526217f27342e175d0e077e263451150104"
ciphertext = bytes.fromhex(ciphertext_hex)

# Step 1: Use the known flag prefix to recover key bytes
known_plaintext = b"crypto{"
key_fragment = bytes(c ^ p for c, p in zip(ciphertext, known_plaintext))
# key_fragment = b'myXORke' — clearly the start of "myXORkey"
print(f"Key fragment: {key_fragment}")  # b'myXORke'

# Step 2: Use the full 8-byte key to decrypt
key = b"myXORkey"
plaintext = bytes(c ^ key[i % len(key)] for i, c in enumerate(ciphertext))
print(f"Flag: {plaintext.decode()}")
```

**Output:**
```
Key fragment: b'myXORke'
Flag: crypto{1f_y0u_Kn0w_En0uGH_y0u_Kn0w_1t_4ll}
```

---

## 🚫 Common Pitfalls

- **Working with strings instead of bytes**: `"crypto{"` won't XOR with bytes — use `b"crypto{"`.
  - *Fix*: Always use byte literals (`b"..."`) or `.encode()` when doing XOR operations.

- **Off-by-one in key cycling**: Forgetting `i % len(key)` and using the raw index.
  - *Fix*: Always use `key[i % len(key)]` to wrap around when the key is shorter than the message.

- **Stopping after 7 key bytes**: The fragment `myXORke` has 7 characters. Without recognising the word `myXORkey`, you'd miss the 8th byte.
  - *Fix*: Look for recognisable patterns in the recovered key fragment — it's usually a real word or phrase.

---

## 🧪 Practice Variations

1. **Single-byte XOR**: What if the key were just 1 byte? How would you brute-force all 256 possibilities? (See [PRACTICE.md](PRACTICE.md) Problem 3)
2. **Longer key, longer plaintext**: Try encrypting a message with key `b"mysecret"` and recovering it with only the first 8 known plaintext bytes.
3. **Cryptopals Challenge**: [Set 1, Challenge 6](https://cryptopals.com/sets/1/challenges/6) — break repeating-key XOR on an unknown key length (uses Hamming distance to find the key length first).
4. **TryHackMe**: [W1seGuy Task 2](../TryHackMe/W1seGuy.md) — the same attack on a live server with a 5-byte random key.

---

## 🌍 Real-World Applications

- **Stream ciphers**: Designs like RC4 and ChaCha20 use XOR to combine keystream bytes with plaintext. If the same keystream is ever reused (nonce reuse), a known-plaintext attack recovers the XOR of two plaintexts.
- **CBC XOR attacks**: In AES-CBC mode, a padding oracle attack exploits XOR relationships between ciphertext blocks.
- **One-Time Pad (OTP)**: The secure version of XOR encryption — the key must be as long as the plaintext, truly random, and never reused. Any of these conditions broken → insecure.

---

## 🔗 Resources

- [CryptoHack — Introduction Track](https://cryptohack.org/courses/intro/)
- [XOR and Cryptography — Cryptopals](https://cryptopals.com/sets/1)
- [CONCEPTS.md — XOR Properties](CONCEPTS.md#xor-properties)
- [TOOLS.md — XOR Snippets](TOOLS.md#xor-operations)

---

## ✅ Flag

```
crypto{1f_y0u_Kn0w_En0uGH_y0u_Kn0w_1t_4ll}
```

---

[🏠 Home](../README.md) | [🔓 CryptoHack](README.md)

*CryptoHack CTF Writeup — andreralf25 — 2026*
