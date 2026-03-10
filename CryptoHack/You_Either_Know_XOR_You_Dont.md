# CTF WRITEUP — CryptoHack: You either know, XOR you don't (Introduction)

**Author:** andreralf25 | **Points Earned:** 30 | **Tasks Completed:** 1

---

## Challenge Overview

This writeup documents the solution to a CryptoHack Introduction challenge. The challenge required decrypting a hex-encoded ciphertext by exploiting a weakness in **XOR encryption with a short repeating key** — specifically a known-plaintext attack using the known flag prefix.

| Property | Details |
|----------|---------|
| **Platform** | CryptoHack |
| **Challenge** | You either know, XOR you don't |
| **Category** | Introduction / XOR |
| **Points** | 30 |

---

## Challenge

The challenge presented the following:

- **Ciphertext (hex):** `0e0b213f26041e480b26217f27342e175d0e070a3c5b103e2526217f27342e175d0e077e263451150104`
- **Description:** "I've encrypted the flag with my secret key, you'll never be able to guess it."
- **Hint:** "Remember the flag format and how it might help you in this challenge!"
- **Flag format:** `crypto{FLAG}`

---

## Vulnerability

XOR with a short repeating key is insecure when any portion of the plaintext is known. Because every CryptoHack flag begins with `crypto{`, we immediately know the first 7 bytes of plaintext. That is enough to recover the first 7 bytes of the key directly — and in this case the key is only 8 bytes long, so we can deduce the full key with minimal effort.

---

## The Math

XOR is its own inverse, giving us this reversible property:

```
plaintext  XOR key = ciphertext
ciphertext XOR plaintext = key
```

Applying it to the known prefix:

```
key[i] = ciphertext[i] XOR plaintext[i]   (for i = 0 … 6)
```

---

## Solution

### Step 1 — Recover the key fragment

XOR the first 7 bytes of ciphertext with the known plaintext `crypto{` (hex `63 72 79 70 74 6f 7b`):

| Byte # | Ciphertext (hex) | Known plaintext | Key byte (hex) | Key byte (ASCII) |
|--------|-----------------|-----------------|----------------|------------------|
| 0 | `0e` | `63` (`c`) | `6d` | `m` |
| 1 | `0b` | `72` (`r`) | `79` | `y` |
| 2 | `21` | `79` (`y`) | `58` | `X` |
| 3 | `3f` | `70` (`p`) | `4f` | `O` |
| 4 | `26` | `74` (`t`) | `52` | `R` |
| 5 | `04` | `6f` (`o`) | `6b` | `k` |
| 6 | `1e` | `7b` (`{`) | `65` | `e` |

Key fragment: **`myXORke`**

### Step 2 — Deduce the full key

The fragment `myXORke` is clearly the start of the word **`myXORkey`** (8 bytes). This is the repeating key used to encrypt the entire ciphertext.

### Step 3 — Decrypt the full ciphertext

XOR every byte of the ciphertext with the repeating key `myXORkey`:

```
plaintext[i] = ciphertext[i] XOR key[i % 8]
```

This produces the plaintext flag.

---

## Solve Script

```python
ciphertext_hex = "0e0b213f26041e480b26217f27342e175d0e070a3c5b103e2526217f27342e175d0e077e263451150104"
ciphertext = bytes.fromhex(ciphertext_hex)

# Recover key using known plaintext "crypto{"
known = b"crypto{"
key_fragment = bytes([c ^ p for c, p in zip(ciphertext, known)])
# key_fragment = b"myXORke" → full key is "myXORkey"

key = b"myXORkey"
plaintext = bytes([c ^ key[i % len(key)] for i, c in enumerate(ciphertext)])
print(plaintext.decode())  # crypto{1f_y0u_Kn0w_En0uGH_y0u_Kn0w_1t_4ll}
```

---

## Flag

```
crypto{1f_y0u_Kn0w_En0uGH_y0u_Kn0w_1t_4ll}
```

---

## Key Takeaways

- **XOR with a short repeating key** is insecure when any plaintext is known.
- **Known-plaintext attacks** recover key bytes directly via XOR's reversibility.
- **Flag formats** like `crypto{...}` leak enough known plaintext to break short XOR keys without any brute force.
- Secure encryption requires keys at least as long as the plaintext (one-time pad) or modern authenticated ciphers like **AES-GCM**.

---

*CryptoHack CTF Writeup — andreralf25 — 2026*
