# 🧪 Practice Problems & Resources

> Additional challenges, variations, and resources to reinforce the concepts from these writeups.

[🏠 Home](../README.md) | [🔓 CryptoHack](README.md)

---

## 📋 Table of Contents

1. [XOR Practice Problems](#xor-practice-problems)
2. [Encoding Practice](#encoding-practice)
3. [Classical Ciphers Practice](#classical-ciphers-practice)
4. [RSA Practice](#rsa-practice)
5. [Recommended CTF Platforms](#recommended-ctf-platforms)
6. [Learning Resources](#learning-resources)

---

## XOR Practice Problems

### Problem 1 — XOR Basics

XOR the following byte arrays and decode the result as ASCII:

```python
a = bytes([0x73, 0x63, 0x72, 0x65, 0x74])
b = bytes([0x00, 0x00, 0x00, 0x00, 0x00])
# What is a XOR b?
```

<details>
<summary>💡 Hint</summary>

XOR with all zeros is the identity operation — `A ⊕ 0 = A`.

</details>

<details>
<summary>✅ Answer</summary>

`b"secret"` — XOR with zero returns the original value unchanged.

</details>

---

### Problem 2 — Self-Inverse Property

```python
key = b"key"
message = b"hello"
# Encrypt
ciphertext = bytes(m ^ key[i % len(key)] for i, m in enumerate(message))
# Can you decrypt it WITHOUT writing extra code? How?
```

<details>
<summary>✅ Answer</summary>

Apply the same XOR operation again — XOR is its own inverse:
```python
recovered = bytes(c ^ key[i % len(key)] for i, c in enumerate(ciphertext))
# recovered == b"hello"
```

</details>

---

### Problem 3 — Known-Plaintext Key Recovery

The ciphertext `bytes.fromhex("1b37373331363f78151b7f2b783431333d78397828372d363c78373e783a393b3736")` was XOR'd with a single repeated byte. What is the key byte and the plaintext?

<details>
<summary>💡 Hint</summary>

Try all 256 possible key bytes (0–255). Score each result by counting printable ASCII characters. The key that produces the most readable English text wins. This is **single-byte XOR brute force**.

</details>

<details>
<summary>✅ Answer</summary>

```python
ciphertext = bytes.fromhex("1b37373331363f78151b7f2b783431333d78397828372d363c78373e783a393b3736")
best = max(range(256), key=lambda k: sum(chr(b ^ k).isalpha() for b in ciphertext))
print(chr(best), bytes(b ^ best for b in ciphertext))
# Key: 88 ('X') → "Cooking MC's like a pound of bacon"
```

This is Cryptopals Set 1, Challenge 3 — a classic!

</details>

---

### Problem 4 — XOR Chain

Given:
```
key1 = 0xa6
key2 = 0xb4
key3 = 0xf6
message = 0x37
```

Compute `message ⊕ key1 ⊕ key2 ⊕ key3`. What is the associativity property telling you here?

<details>
<summary>✅ Answer</summary>

```python
result = 0x37 ^ 0xa6 ^ 0xb4 ^ 0xf6  # = 0x01
# Or equivalently:
chain_key = 0xa6 ^ 0xb4 ^ 0xf6       # = 0x04
result = 0x37 ^ chain_key             # = 0x33
```

Associativity means you can XOR all keys together first, then apply once — useful for optimising multi-key XOR chains.

</details>

---

## Encoding Practice

### Problem 5 — Hex Decode

Decode this hex string to ASCII:
```
43 72 79 70 74 6f 48 61 63 6b
```

<details>
<summary>✅ Answer</summary>

```python
bytes.fromhex("43727970746f4861636b").decode()  # "CryptoHack"
```

</details>

---

### Problem 6 — Base64 Round-Trip

```python
import base64
original = b"The quick brown fox"
encoded = base64.b64encode(original)
# What is `encoded`? How many `=` padding characters appear, and why?
```

<details>
<summary>✅ Answer</summary>

```python
# encoded = b'VGhlIHF1aWNrIGJyb3duIGZveA=='
# 2 padding `=` characters because len(b"The quick brown fox") = 19
# 19 % 3 = 1 remaining byte → needs 2 padding characters
```

</details>

---

### Problem 7 — Big Integer Conversion

Convert the message `"CryptoHack"` to a big integer, then back to bytes. Confirm you recover the original.

<details>
<summary>✅ Answer</summary>

```python
msg = b"CryptoHack"
n = int.from_bytes(msg, 'big')     # 89505604786721487882506
recovered = n.to_bytes(10, 'big')  # b'CryptoHack'
assert recovered == msg
```

</details>

---

## Classical Ciphers Practice

### Problem 8 — ROT13 Variation

What is ROT7 of `"Hello, CTF!"`? (Only rotate letters, shift by 7.)

<details>
<summary>✅ Answer</summary>

```python
def rot_n(text, n):
    result = []
    for ch in text:
        if ch.isalpha():
            base = ord('A') if ch.isupper() else ord('a')
            result.append(chr((ord(ch) - base + n) % 26 + base))
        else:
            result.append(ch)
    return ''.join(result)

rot_n("Hello, CTF!", 7)  # "Olssv, JAM!"
```

</details>

---

### Problem 9 — A1Z26

Decode: `8 5 12 12 15 { 23 15 18 12 4 }`

<details>
<summary>✅ Answer</summary>

`HELLO{WORLD}` — A=1, B=2, … Z=26.

</details>

---

## RSA Practice

### Problem 10 — Small Prime Factor

Given `N = 77, e = 7, c = 57`, decrypt the ciphertext.

*(Hint: factor N by hand, then compute φ(N) and d.)*

<details>
<summary>✅ Answer</summary>

```python
N, e, c = 77, 7, 57
# 77 = 7 * 11
p, q = 7, 11
phi = (p - 1) * (q - 1)  # = 60
d = pow(e, -1, phi)       # = pow(7, -1, 60) = 43
m = pow(c, d, N)          # = pow(57, 43, 77) = 10
```

</details>

---

## Recommended CTF Platforms

| Platform | Level | Focus | Link |
|----------|-------|-------|------|
| **CryptoHack** | Beginner → Advanced | Pure cryptography | [cryptohack.org](https://cryptohack.org) |
| **picoCTF** | Beginner → Intermediate | All categories, archived challenges | [picoctf.org](https://picoctf.org) |
| **TryHackMe** | Beginner | Guided learning rooms | [tryhackme.com](https://tryhackme.com) |
| **Cryptopals** | Intermediate → Advanced | Real-world crypto attacks | [cryptopals.com](https://cryptopals.com) |
| **HackTheBox** | Intermediate → Advanced | All categories | [hackthebox.com](https://app.hackthebox.com) |
| **CTFtime** | All levels | Competition calendar & archives | [ctftime.org](https://ctftime.org) |

---

## Learning Resources

### Books
- *"Serious Cryptography"* — Jean-Philippe Aumasson (No Starch Press)
- *"The Code Book"* — Simon Singh (great for history + classical ciphers)
- *"Applied Cryptography"* — Bruce Schneier

### Online Courses
- [Dan Boneh's Cryptography I](https://www.coursera.org/learn/crypto) — Stanford, free on Coursera
- [CryptoHack Learning Platform](https://cryptohack.org) — Hands-on challenges

### Video Resources
- [LiveOverflow CTF Tutorials](https://www.youtube.com/c/LiveOverflow) — YouTube
- [0xdf CTF Walkthroughs](https://0xdf.gitlab.io/) — Blog

---

[🏠 Home](../README.md) | [🔓 CryptoHack](README.md) | [📖 Concepts](CONCEPTS.md)
