# EVEN RSA CAN BE BROKEN???

| Detail       | Value                          |
|--------------|--------------------------------|
| **CTF**      | picoCTF 2025                   |
| **Category** | Cryptography                   |
| **Difficulty** | Easy                         |
| **Author**   | Michael Crotty                 |
| **Solves**   | 21,437 (95% liked)             |
| **Tags**     | `Cryptography`, `picoCTF 2025`, `browser_webshell_solvable` |

---

## 📝 Description

> This service provides you an encrypted flag. Can you decrypt it with just N & e?
>
> Connect to the program with netcat:
> ```
> $ nc verbal-sleep.picoctf.net 60275
> ```
> The program's source code can be downloaded [here].

---

## 🧩 Challenge Overview

After connecting to the program via netcat, it provides us with three values:

- **N** — the RSA modulus
- **e** — the public exponent (65537)
- **ciphertext** — the encrypted flag

Our goal: decrypt the ciphertext to recover the flag.

---

## 💡 Background: What is RSA?

**RSA** (Rivest–Shamir–Adleman) is one of the most widely used public-key cryptosystems. Here's a quick refresher:

### Key Generation

1. Pick two large prime numbers: `p` and `q`
2. Compute the modulus: `n = p × q`
3. Compute Euler's totient: `φ(n) = (p - 1)(q - 1)`
4. Choose a public exponent `e` (commonly 65537) such that `gcd(e, φ(n)) = 1`
5. Compute the private exponent: `d = e⁻¹ mod φ(n)`

| Key | Components | Purpose |
|-----|-----------|---------|
| 🔓 Public Key  | `(n, e)` | Used to **encrypt** |
| 🔒 Private Key | `(n, d)` | Used to **decrypt** |

### Encryption & Decryption

- **Encrypt:** `c = m^e mod n`
- **Decrypt:** `m = c^d mod n`

The security of RSA relies on the difficulty of factoring `n` back into `p` and `q`. But what if one of those primes is... *trivially small*?

---

## 🧰 Tools Used

- **netcat (`nc`)** — to connect to the challenge server
- **Python 3** — for scripting the decryption
- **Any math evaluator / big-integer calculator** — for modular arithmetic

---

## 🚶 Walkthrough

### Step 1: Connect & Gather Values

Connect to the challenge server:

```bash
nc verbal-sleep.picoctf.net 60275
```

The server outputs something like:

```
N: 2507945537099368400123217521...
e: 65537
cyphertext: 23923161513273733552066557...
```

> ⚠️ Your values will be different each time you connect — a new ciphertext is generated on every run.

### Step 2: Examine the Source Code

The provided source code reveals how the encryption works:

```python
from sys import exit
from Crypto.Util.number import bytes_to_long, inverse
from setup import get_primes

e = 65537

def gen_key(k):
    """
    Generates RSA key with k bits
    """
    p, q = get_primes(k // 2)
    N = p * q
    d = inverse(e, (p - 1) * (q - 1))
    return ((N, e), d)

def encrypt(pubkey, m):
    N, e = pubkey
    return pow(bytes_to_long(m.encode('utf-8')), e, N)

def main(flag):
    pubkey, _privkey = gen_key(1024)
    encrypted = encrypt(pubkey, flag)
    return (pubkey[0], encrypted)

if __name__ == "__main__":
    flag = open('flag.txt', 'r').read()
    flag = flag.strip()
    N, cypher = main(flag)
    print("N:", N)
    print("e:", e)
    print("cyphertext:", cypher)
    exit()
```

This is standard RSA — **but the vulnerability is in `get_primes()`**. We don't have the source for `setup.py`, but the hint is in the challenge title itself: **"EVEN RSA CAN BE BROKEN???"**

### Step 3: Spot the Vulnerability — N is EVEN

The key insight: **N is an even number.**

In RSA, `n = p × q` where both `p` and `q` should be large primes. But `2` is the **only even prime number**. So if `N` is even, then one of the factors **must** be `2`.

This means:

```
p = 2
q = N / 2
```

This completely breaks RSA because factoring `N` becomes trivial!

### Step 4: Compute φ(n)

```
φ(n) = (p - 1)(q - 1)
     = (2 - 1)(q - 1)
     = q - 1
     = (N / 2) - 1
```

### Step 5: Compute the Private Key `d`

```
d = e⁻¹ mod φ(n)
d = inverse(65537, q - 1)
```

### Step 6: Decrypt the Ciphertext

```
m = c^d mod N
```

Where:
- `m` = the original plaintext message
- `c` = the ciphertext
- `d` = the private exponent
- `N` = the RSA modulus

### Step 7: Solve Script (Python)

Here's a complete solve script:

```python
from Crypto.Util.number import long_to_bytes, inverse

# Paste your values from the server here
N = <your_N_value>
e = 65537
c = <your_ciphertext_value>

# N is even, so p = 2
p = 2
q = N // 2

# Compute Euler's totient
phi_n = (p - 1) * (q - 1)

# Compute private key
d = inverse(e, phi_n)

# Decrypt
m = pow(c, d, N)

# Convert to bytes and print
print(long_to_bytes(m).decode())
```

### Step 8: Get the Flag 🏁

After running the script with the values from the server:

```
picoCTF{tw0_1$_pr!m3993b4dd0}
```

---

## 🔑 Key Takeaways

1. **RSA is only secure when both primes are large and random.** Using `2` as a prime factor makes `N` trivially factorable.
2. **Always check if N is even.** If it is, one factor is `2` and the problem collapses.
3. **The challenge title is a hint!** "EVEN RSA" → the modulus is *even* → one prime is `2`.
4. **Understanding the math behind RSA** is essential for cryptography CTF challenges.

---

## 🏷️ Flag

```
picoCTF{tw0_1$_pr!m3993b4dd0}
```