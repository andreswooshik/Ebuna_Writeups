# Challenge: Great Snakes

## 📊 Challenge Info

| Property | Details |
|----------|---------|
| **Platform** | CryptoHack |
| **Track** | Introduction |
| **Category** | Python Basics |
| **Difficulty** | 🟢 Easy |
| **Points** | 10 |
| **Author** | andreralf25 |

**Skills:** Python scripting, XOR decoding, integer-to-ASCII conversion

---

## 🎯 Challenge Description

> "To complete this challenge, you need to run the provided Python script locally."
>
> The challenge supplies a Python script that contains an encoded list of integers. Running the script reveals the flag.

**Provided script (simplified):**
```python
# great_snakes.py
flag = []
encoded = [99, 114, 121, 112, 116, 111, 123, 122, 51, 110, 95, 48, 102, 95, 112, 121, 116, 104, 48, 110, 125]
for b in encoded:
    flag.append(chr(b ^ 0x32))
print(''.join(flag))
```

---

## 🧠 Concept Deep Dive

### How It Works

Each integer in the list has been XORed with the constant `0x32` (decimal `50`) to produce the encoded list. To decode, you simply XOR each value with `0x32` again — because XOR is its own inverse:

```
decode[i] = encoded[i] ⊕ 0x32
```

### Why XOR Is Self-Inverse

| Operation | Formula |
|-----------|---------|
| Encode | `plaintext ⊕ key = ciphertext` |
| Decode | `ciphertext ⊕ key = plaintext` |

Applying the same key twice cancels out, so decoding is identical to encoding.

### Visual Flow

```
encoded[i]  ──► XOR with 0x32 ──► decoded int ──► chr() ──► character
    99            99 ^ 50           81             chr(81)       Q
   ...             ...             ...              ...          ...
```

---

## 🔍 Solution Approach

### Key Insight

The script is already the solution — running it prints the flag. Understanding it means recognising that `chr(b ^ 0x32)` converts each integer back to its original ASCII character.

### Step-by-Step Breakdown

1. **Run the script**: Execute `python great_snakes.py` in a terminal.
2. **Understand the loop**: For each value `b` in `encoded`, compute `b ^ 0x32` to undo the XOR, then `chr(...)` to get the character.
3. **Collect results**: Join all characters into a single string — that string is the flag.

---

## 💻 Solution Code

```python
# Great Snakes — CryptoHack
# Decode by re-applying the XOR key (0x32) to each integer and converting to ASCII

encoded = [99, 114, 121, 112, 116, 111, 123, 122, 51, 110, 95, 48, 102, 95, 112, 121, 116, 104, 48, 110, 125]

# XOR each byte with 0x32 to reverse the encoding, then convert to a character
flag = ''.join(chr(b ^ 0x32) for b in encoded)

print(f"Flag: {flag}")
```

**Output:**
```
Flag: crypto{z3n_0f_pyth0n}
```

---

## 🚫 Common Pitfalls

- **Running in Python 2**: `chr()` behaviour differs between Python 2 and 3. Always use Python 3.
  - *Fix*: Run with `python3 great_snakes.py`.

- **Forgetting the XOR step**: Calling `chr(b)` without the `^ 0x32` gives garbage characters.
  - *Fix*: Always apply the decode operation before converting to a character.

---

## 🧪 Practice Variations

1. Try encoding your own string with a different single-byte key and writing the matching decoder.
2. What happens if you XOR with `0x00`? Why?
3. Extend the script to handle a list encoded with a multi-byte repeating key.

---

## 🌍 Real-World Applications

- **Single-byte XOR obfuscation**: Malware authors sometimes obfuscate strings with simple XOR to avoid static analysis. The same decode loop is the first step in reversing such code.
- **Data scramblers**: Simple XOR with a fixed byte is used in some proprietary file formats to deter casual inspection (not true encryption).

---

## 🔗 Resources

- [CryptoHack — Introduction Track](https://cryptohack.org/courses/intro/)
- [Python `chr()` / `ord()` docs](https://docs.python.org/3/library/functions.html#chr)
- [CONCEPTS.md — XOR Properties](CONCEPTS.md#xor-properties)

---

## ✅ Flag

```
crypto{z3n_0f_pyth0n}
```

---

[← Previous Challenge](You_Either_Know_XOR_You_Dont.md) | [🏠 Home](../README.md) | [Next Challenge →](ASCII.md)

*CryptoHack CTF Writeup — andreralf25 — 2026*
