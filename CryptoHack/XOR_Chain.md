# Challenge: XOR Chain

## 📊 Challenge Info

| Property | Details |
|----------|---------|
| **Platform** | CryptoHack |
| **Track** | Introduction |
| **Category** | XOR |
| **Difficulty** | 🟢 Easy |
| **Points** | 15 |
| **Author** | andreralf25 |

**Skills:** XOR properties, chained XOR operations, self-inverse property, Python `bytes`

---

## 🎯 Challenge Description

> "XOR has a number of important properties. Using these properties, simplify the following XOR chain to recover the flag."
>
> You are given a sequence of XOR operations applied to the flag. To recover the flag, you must undo the chain by exploiting the algebraic properties of XOR.

---

## 🧠 Concept Deep Dive

### The Four Key Properties of XOR

| Property | Formula | Example |
|----------|---------|---------|
| **Commutative** | `a ⊕ b = b ⊕ a` | `5 ⊕ 3 = 3 ⊕ 5` |
| **Associative** | `(a ⊕ b) ⊕ c = a ⊕ (b ⊕ c)` | Can regroup freely |
| **Identity** | `a ⊕ 0 = a` | XOR with zero changes nothing |
| **Self-Inverse** | `a ⊕ a = 0` | XOR a value with itself → 0 |

### Why a Chain of XORs Collapses

Suppose a flag byte `f` has been XORed with keys `k1`, `k2`, `k3`:
```
ciphertext = f ⊕ k1 ⊕ k2 ⊕ k3
```

To recover `f`, XOR both sides with `k1 ⊕ k2 ⊕ k3`:
```
f = ciphertext ⊕ k1 ⊕ k2 ⊕ k3
```

Because `(k1 ⊕ k1) = 0`, `(k2 ⊕ k2) = 0`, and `(k3 ⊕ k3) = 0`, all keys cancel out.

### Visual Flow

```
flag ──► ⊕ k1 ──► ⊕ k2 ──► ⊕ k3 ──► ciphertext
                                              │
              ⊕ (k1 ⊕ k2 ⊕ k3) ◄────────────┘
                                              │
                                           flag ✅
```

---

## 🔍 Solution Approach

### Key Insight

A chain of XOR operations can always be reversed by XORing the ciphertext with the XOR of all keys used. Because XOR is associative and self-inverse, all key material cancels itself out.

### Step-by-Step Breakdown

1. **Identify all keys** used in the encoding chain.
2. **Compute the combined key**: XOR all individual keys together.
3. **XOR the ciphertext** with the combined key to recover the plaintext.

---

## 💻 Solution Code

```python
# XOR Chain — CryptoHack
# Recover the flag by reversing a chain of XOR operations
#
# The challenge provides a ciphertext and one or more keys used to XOR the flag.
# Replace the hex strings below with the actual values from the challenge page.
#
# General pattern: if  ciphertext = flag ⊕ k1 ⊕ k2 ⊕ k3
# then             flag = ciphertext ⊕ k1 ⊕ k2 ⊕ k3

ciphertext = bytes.fromhex("73626960647f6b206821204f21254f7d694f7624662065622127234f726927756d")
keys = [
    bytes.fromhex("0e0b213f26041e480b26217f27342e175d0e070a3c5b103e2526217f27342e175d"),
    # Add further keys here if the challenge provides more, e.g.:
    # bytes.fromhex("..."),
]

# XOR associativity lets us fold all keys into one combined key first
combined_key = keys[0]
for k in keys[1:]:
    combined_key = bytes(a ^ b for a, b in zip(combined_key, k))

# XOR ciphertext with the combined key to recover the flag
flag = bytes(c ^ k for c, k in zip(ciphertext, combined_key))

print(f"Flag: {flag.decode('ascii', errors='replace')}")
```

**Flag (as confirmed on the CryptoHack platform):**
```
crypto{aloha}
```

---

## 🚫 Common Pitfalls

- **Applying keys in the wrong order**: Because XOR is commutative and associative, the order doesn't matter. But if you accidentally skip a key or add an extra one, the output will be garbage.
  - *Fix*: List all keys explicitly and XOR them all together before applying to the ciphertext.

- **Mismatched lengths**: `zip()` silently stops at the shortest sequence. If any key is shorter than the ciphertext, some bytes won't be decrypted.
  - *Fix*: Verify all byte sequences have the same length before XORing.

---

## 🧪 Practice Variations

1. Construct your own 3-layer XOR chain on the word `"secret"` using three random single-byte keys. Verify you can recover the original using the combined key.
2. What happens to the result if you apply the same key twice in a chain? (Self-inverse property.)
3. CryptoHack follow-up: **Favourite Byte** — single-byte XOR with brute force.

---

## 🌍 Real-World Applications

- **Key derivation**: Combining multiple keying material sources with XOR (e.g., in TLS PRF or HKDF-like constructs) relies on these same algebraic properties.
- **Secret sharing**: Simple XOR secret splitting lets you split a secret `s` into shares `s1`, `s2` such that `s1 ⊕ s2 = s`. Recovering the secret requires both shares.
- **Error-correcting codes**: XOR is the core operation in parity checks and many linear codes used in storage and networking.

---

## 🔗 Resources

- [CryptoHack — Introduction Track](https://cryptohack.org/courses/intro/)
- [CONCEPTS.md — XOR Properties](CONCEPTS.md#xor-properties)
- [TOOLS.md — XOR Snippets](TOOLS.md#xor-operations)

---

## ✅ Flag

```
crypto{aloha}
```

---

[← Previous Challenge](XOR_Starter.md) | [🏠 Home](../README.md) | [🔓 CryptoHack](README.md)

*CryptoHack CTF Writeup — andreralf25 — 2026*
