# CTF WRITEUP — TryHackMe: W1seGuy (Cryptography Room)

**Author:** andreralf25 | **Points Earned:** 60 | **Tasks Completed:** 2

---

## Challenge Overview

This writeup documents the solution to a two-part TryHackMe cryptography challenge. The challenge required decrypting encoded flags by exploiting weaknesses in classical ciphers — specifically a **Caesar Cipher** and an **XOR cipher** with a short repeating key.

| Property | Details |
|----------|---------|
| **Platform** | TryHackMe |
| **Room** | W1seGuy |
| **Tasks** | 2 (Caesar Cipher + XOR Known Plaintext) |
| **Points Earned** | 60 |
| **Target IP** | `10.48.143.100` |
| **Port** | `1337` (TCP) |

---

## Task 1 — Caesar Cipher

### Challenge

The challenge presented the ciphertext `XRPCTCRGNEI` and asked for the original plaintext.

### Approach

A Caesar cipher shifts each letter by a fixed number of positions. Since there are only 25 possible shifts, we can brute force all of them.

### Solution

Brute forcing all 25 shifts, **shift 15** produced the only meaningful English result:

| Shift | Result | Valid? |
|-------|--------|--------|
| 1 | WQOBSBQFMDH | No |
| ... | ... | ... |
| **15** | **ICANENCRYPT** | **YES ✓** |
| 16–25 | (gibberish) | No |

### Flag 1

```
THM{p1alntExtAtt4ckcAnr3alLyhUrty0urxOr}
```

---

## Task 2 — XOR Known Plaintext Attack

### Challenge

A server at `10.48.143.100:1337` sends a hex-encoded XOR ciphertext and asks for the encryption key. The key is a random 5-character alphanumeric string regenerated each session.

### Vulnerability

XOR with a short repeating key is weak when any part of the plaintext is known. Since every flag follows the format `THM{...}`, we know the first 4 bytes of plaintext.

### The Math

XOR has a reversible property:

```
plaintext  XOR key = ciphertext
ciphertext XOR plaintext = key
```

Using the known prefix `THM{` we can recover 4 of the 5 key characters directly:

```
key[0] = ciphertext[0] XOR 'T'
key[1] = ciphertext[1] XOR 'H'
key[2] = ciphertext[2] XOR 'M'
key[3] = ciphertext[3] XOR '{'
```

The 5th character was brute forced from 62 alphanumeric candidates, validating that the decrypted output ends with `}`.

### Exploit Script

The following Python script connects to the server, cracks the key, and retrieves Flag 2 in a single session:

```python
import socket, string

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect(("10.48.143.100", 1337))
data = s.recv(4096).decode()
xored = bytes.fromhex(data.split("flag 1: ")[1].strip())
known = b"THM{"
key_chars = [chr(xored[i] ^ known[i]) for i in range(4)]
for c in string.ascii_letters + string.digits:
    key = ''.join(key_chars) + c
    dec = ''.join(chr(xored[i] ^ ord(key[i%5])) for i in range(len(xored)))
    if dec.startswith("THM{") and dec.endswith("}"):
        s.recv(4096)
        s.send((key + "\n").encode())
        print(s.recv(4096).decode())
        break
```

### Result

Running the script against the live server yielded both flags. Flag 2 was returned by the server upon submitting the correct key.

| Flag | Value |
|------|-------|
| **Flag 1** | `THM{p1alntExtAtt4ckcAnr3alLyhUrty0urxOr}` |
| **Flag 2** | *(retrieved from server at runtime)* |

---

## Key Takeaways

- **Caesar cipher** is trivially broken by brute forcing all 25 shifts.
- **XOR with a short repeating key** is insecure when any plaintext is known.
- **Known plaintext attacks** recover key bytes directly via XOR's reversibility.
- Secure encryption requires keys at least as long as the plaintext (one-time pad) or modern authenticated ciphers like **AES-GCM**.
- **Flag format** (`THM{...}`) leaks enough plaintext to break the 5-character XOR key with only 62 brute force attempts.

---

## 🧪 Practice Variations

1. **Longer flag format**: What if the flag were `THM{...}` but 10 characters long? How many brute-force attempts for the remaining 6 key characters? (62⁶ ≈ 56 billion — you'd need a smarter approach.)
2. **Unknown flag prefix**: How would you crack this if the flag format were unknown? (Frequency analysis on the decrypted text.)
3. **CryptoHack**: [You Either Know, XOR You Don't](../CryptoHack/You_Either_Know_XOR_You_Dont.md) — the same known-plaintext XOR attack on CryptoHack.

## 🌍 Real-World Applications

- **TLS session keys**: If the same XOR keystream is reused (nonce reuse in stream ciphers), known-plaintext recovers the XOR of both plaintexts.
- **Weak obfuscation**: Many simple "encryption" schemes in malware and IoT firmware use single-byte or short repeating XOR — trivially broken with this technique.

---

[🏠 Home](../README.md)

*TryHackMe CTF Writeup — andreralf25 — 2026*
