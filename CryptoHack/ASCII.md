# Challenge: ASCII

## 📊 Challenge Info

| Property | Details |
|----------|---------|
| **Platform** | CryptoHack |
| **Track** | Introduction |
| **Category** | Encoding |
| **Difficulty** | 🟢 Easy |
| **Points** | 5 |
| **Author** | andreralf25 |

**Skills:** ASCII encoding, Python `chr()`, integer-to-character conversion

---

## 🎯 Challenge Description

> "ASCII is a 7-bit character encoding standard that maps decimal numbers to printable characters."
>
> Convert the following list of decimal ASCII values to a string:
>
> ```
> [99, 114, 121, 112, 116, 111, 123, 65, 83, 67, 73, 73, 95, 112, 114, 49, 110, 116, 52, 98, 108, 51, 125]
> ```

---

## 🧠 Concept Deep Dive

### What is ASCII?

ASCII (American Standard Code for Information Interchange) assigns a unique number (0–127) to every printable character, digit, and common symbol. For example:

| Character | Decimal | Hex |
|-----------|---------|-----|
| `A` | 65 | 0x41 |
| `z` | 122 | 0x7A |
| `{` | 123 | 0x7B |
| `}` | 125 | 0x7D |

Any text on your screen is ultimately stored as a sequence of these numbers in memory.

### How It Works

Python's built-in `chr()` function converts a decimal integer to its ASCII character. The inverse is `ord()`, which converts a character back to its decimal value.

```
chr(65)  →  'A'
ord('A') →  65
```

### Visual Flow

```
[99, 114, 121, ...]
       ↓  chr() on each element
['c', 'r', 'y', ...]
       ↓  ''.join(...)
"crypto{...}"
```

---

## 🔍 Solution Approach

### Key Insight

Each integer in the list is an ASCII code point. Calling `chr()` on every value and joining the results reconstructs the original string.

### Step-by-Step Breakdown

1. **Identify the encoding**: The numbers are in the printable ASCII range (32–126), so each maps directly to a character.
2. **Convert**: Apply `chr()` to each integer.
3. **Join**: Concatenate all characters to form the flag string.

---

## 💻 Solution Code

```python
# ASCII — CryptoHack
# Convert a list of decimal ASCII values to a readable string

values = [99, 114, 121, 112, 116, 111, 123, 65, 83, 67, 73, 73, 95, 112, 114, 49, 110, 116, 52, 98, 108, 51, 125]

# chr() maps each integer to its ASCII character
flag = ''.join(chr(v) for v in values)

print(f"Flag: {flag}")
```

**Output:**
```
Flag: crypto{ASCII_pr1nt4bl3}
```

---

## 🚫 Common Pitfalls

- **Using `bytes(values)`**: This creates a bytes object, not a string. You still need `.decode('ascii')` or use `chr()`.
  - *Fix*: Use `''.join(chr(v) for v in values)` for a direct string, or `bytes(values).decode()`.

- **Values outside printable range**: Numbers below 32 or above 126 are control characters and won't print correctly.
  - *Fix*: Verify all values are in the 32–126 range before calling `chr()`.

---

## 🧪 Practice Variations

1. Reverse the operation: convert `"hello"` to its ASCII decimal values using `ord()`.
2. What is the ASCII code for the space character (`' '`)? Why is it 32?
3. CryptoHack follow-up: [HEX](HEX.md) — encoding the same data in base-16.

---

## 🌍 Real-World Applications

- **Network protocols**: HTTP headers, SMTP email, and many other protocols transmit text as ASCII (or UTF-8, which is backward-compatible with ASCII for the 0–127 range).
- **Forensics**: When analysing binary files or memory dumps, looking for sequences in the ASCII printable range is a common way to extract embedded strings.

---

## 🔗 Resources

- [CryptoHack — Introduction Track](https://cryptohack.org/courses/intro/)
- [ASCII Table](https://www.asciitable.com/)
- [Python `chr()` docs](https://docs.python.org/3/library/functions.html#chr)

---

## ✅ Flag

```
crypto{ASCII_pr1nt4bl3}
```

---

[← Previous Challenge](Great_Snakes.md) | [🏠 Home](../README.md) | [Next Challenge →](HEX.md)

*CryptoHack CTF Writeup — andreralf25 — 2026*
