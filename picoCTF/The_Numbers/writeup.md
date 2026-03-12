# The Numbers

| Detail       | Value                          |
|--------------|--------------------------------|
| **CTF**      | picoCTF 2019                   |
| **Category** | Cryptography                   |
| **Difficulty** | Easy                         |
| **Author**   | Danny                          |
| **Solves**   | 114,933 (67% liked)            |
| **Tags**     | `Easy`, `Cryptography`, `picoCTF 2019` |

---

## 📝 Description

> The numbers... what do they mean?
>
> Download: [numbers.png](numbers.png)

---

## 🧩 Challenge Overview

We're given an image file (`numbers.png`) containing a sequence of numbers and some curly braces. The challenge asks us to figure out what the numbers represent. Spoiler: each number maps to a letter of the alphabet (A=1, B=2, ... Z=26).

---

## 💡 Background: A1Z26 Cipher

The **A1Z26 cipher** (also known as the "letter-number" cipher) is one of the simplest substitution ciphers. Each letter of the alphabet is replaced by its position number:

```
A = 1    B = 2    C = 3    D = 4    E = 5
F = 6    G = 7    H = 8    I = 9    J = 10
K = 11   L = 12   M = 13   N = 14   O = 15
P = 16   Q = 17   R = 18   S = 19   T = 20
U = 21   V = 22   W = 23   X = 24   Y = 25   Z = 26
```

---

## 🧰 Tools Used

- **Python 3** — custom script to automate the conversion

---

## 🚶 Walkthrough

### Step 1: Examine the Image

Open `numbers.png`. It contains the following sequence:

```
16 9 3 15 3 20 6 { 20 8 5
14 21 13 2 5 18 19 13 1
19 15 14 }
```

Notice the `{` and `}` characters — this looks like a flag format! The numbers between and around the braces likely map to letters.

### Step 2: Map Numbers to Letters

Reading the numbers in order and converting each to its corresponding letter (1=A, 2=B, ... 26=Z):

| Number | Letter |
|--------|--------|
| 16 | P |
| 9  | I |
| 3  | C |
| 15 | O |
| 3  | C |
| 20 | T |
| 6  | F |
| {  | { |
| 20 | T |
| 8  | H |
| 5  | E |
| 14 | N |
| 21 | U |
| 13 | M |
| 2  | B |
| 5  | E |
| 18 | R |
| 19 | S |
| 13 | M |
| 1  | A |
| 19 | S |
| 15 | O |
| 14 | N |
| }  | } |

Putting it all together: **PICOCTF{THENUMBERSMASON}**

### Step 3: Solve Script (Python)

Here's the Python script used to solve it:

```python

def convert_to_text(numbers):
    letters = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"  # Alphabet string
    result = ""

    for item in numbers:
        # Check if the item is a number; convert to letter if so
        if isinstance(item, int):
            result += letters[item - 1]  # Convert number to corresponding letter
        else:
            result += str(item)  # Add non-integer items directly (like { and })

    return result

# Given list from the challenge image
list_to_convert = [
    16, 9, 3, 15, 3, 20, 6, "{", 
    20, 8, 5, 14, 21, 13, 2, 5, 
    18, 19, 13, 1, 19, 15, 14, "}"
]

converted_text = convert_to_text(list_to_convert)
print(converted_text)
```

**Output:**
```
PICOCTF{THENUMBERSMASON}
```

### Step 4: Get the Flag 🏁

```
PICOCTF{THENUMBERSMASON}
```

---

## 🔑 Key Takeaways

1. **A1Z26 is one of the most basic ciphers.** If you see numbers in the range 1–26, always try mapping them to alphabet positions first.
2. **Look for structure.** The `{` and `}` characters in the image immediately signal a flag format, confirming the cipher approach.
3. **Pop culture reference!** "The numbers, Mason — what do they mean?" is an iconic quote from *Call of Duty: Black Ops* (2010). The flag `THENUMBERSMASON` is a nod to this.
4. **Python makes it easy.** A simple script with a list and alphabet string can decode this in seconds.

---

## 🏷️ Flag

```
PICOCTF{THENUMBERSMASON}
```
---

[🏠 Home](../../README.md) | [→ Next: Mod 26](../Mod_26/writeup.md)
