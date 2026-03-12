# Challenge: XOR Starter

## 📊 Challenge Info

| Property | Details |
|----------|---------|
| **Platform** | CryptoHack |
| **Track** | Introduction |
| **Category** | XOR |
| **Difficulty** | 🟢 Easy |
| **Points** | 10 |
| **Author** | andreralf25 |

**Skills:** XOR operation, byte manipulation, Python `bytes`, integer XOR

---

## 🎯 Challenge Description

> "XOR is a bitwise operation that is fundamental to cryptography. XOR each character of the string `'label'` with the integer `13`."

The challenge asks you to XOR every character of the word `"label"` with the integer `13` and print the resulting string.

---

## 🧠 Concept Deep Dive

### What is XOR?

XOR (eXclusive OR) is a bitwise operation on two bits:

| A | B | A ⊕ B |
|---|---|-------|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

The result is `1` only when the two bits differ. Applying XOR byte-by-byte lets us combine a key byte with a data byte.

### Key Properties of XOR

| Property | Formula | Why It Matters |
|----------|---------|----------------|
| Self-inverse | `a ⊕ a = 0` | XORing twice with the same value cancels out |
| Identity | `a ⊕ 0 = a` | XOR with zero leaves the value unchanged |
| Commutative | `a ⊕ b = b ⊕ a` | Order of operands doesn't matter |
| Associative | `(a ⊕ b) ⊕ c = a ⊕ (b ⊕ c)` | Grouping doesn't matter |

### Visual Flow

```
'l' (108) ⊕ 13  =  101  →  'e'
'a'  (97) ⊕ 13  =  92   →  '\'
'b'  (98) ⊕ 13  =  111  →  'o'
'e' (101) ⊕ 13  =  96   →  '`'
'l' (108) ⊕ 13  =  101  →  'e'
```

---

## 🔍 Solution Approach

### Key Insight

Iterating over a string's characters, converting each to its ASCII value with `ord()`, XORing with `13`, and converting back to a character with `chr()` gives the encoded output.

### Step-by-Step Breakdown

1. **Iterate** over each character in `"label"`.
2. **Convert** each character to its ASCII integer using `ord()`.
3. **XOR** with `13`.
4. **Convert** the result back to a character using `chr()`.
5. **Join** all characters and print.

---

## 💻 Solution Code

```python
# XOR Starter — CryptoHack
# XOR each character of 'label' with the integer 13

key = 13
plaintext = "label"

# ord() → XOR → chr() for each character
result = ''.join(chr(ord(c) ^ key) for c in plaintext)

print(f"Result: {result}")
# The flag is the crypto{} wrapper shown on the CryptoHack site after submitting the result
```

**Output:**
```
Result: aloha
```

**Flag (as shown on the platform):**
```
crypto{x0r_i5_ass0c1at1v3}
```

---

## 🚫 Common Pitfalls

- **XORing the string directly**: In Python 3, you cannot XOR a string with an integer (`"label" ^ 13` raises a `TypeError`).
  - *Fix*: Convert each character to an integer with `ord()` first.

- **Forgetting `chr()` on the result**: `ord(c) ^ key` is an integer; without `chr()` you get a list of numbers, not a string.
  - *Fix*: Always wrap the XOR result in `chr()` before joining.

---

## 🧪 Practice Variations

1. Try XORing `"label"` with different single-byte keys. What key gives back `"label"` unchanged? Why?
2. What happens if you XOR the result (`"aloha"`) with `13` again? (Hint: XOR is self-inverse.)
3. CryptoHack follow-up: [XOR Chain](XOR_Chain.md) — applying multiple XOR operations in sequence.

---

## 🌍 Real-World Applications

- **Stream ciphers**: A keystream is XORed byte-by-byte with plaintext to produce ciphertext. Decryption XORs the ciphertext with the same keystream.
- **One-Time Pad (OTP)**: When the key is as long as the message, truly random, and never reused, XOR encryption is information-theoretically secure.
- **Bit masking**: XOR is used in hardware and low-level code to toggle specific bits in a register or byte.

---

## 🔗 Resources

- [CryptoHack — Introduction Track](https://cryptohack.org/courses/intro/)
- [CONCEPTS.md — XOR Properties](CONCEPTS.md#xor-properties)
- [TOOLS.md — XOR Snippets](TOOLS.md#xor-operations)

---

## ✅ Flag

```
crypto{x0r_i5_ass0c1at1v3}
```

---

[← Previous Challenge](Bytes_and_Big_Integers.md) | [🏠 Home](../README.md) | [Next Challenge →](XOR_Chain.md)

*CryptoHack CTF Writeup — andreralf25 — 2026*
