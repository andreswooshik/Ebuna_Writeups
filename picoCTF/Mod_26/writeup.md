# Mod 26

| Detail       | Value                          |
|--------------|--------------------------------|
| **CTF**      | picoCTF 2021                   |
| **Category** | Cryptography                   |
| **Difficulty** | Easy                         |
| **Author**   | Pandu                          |
| **Solves**   | 293,389 (93% liked)            |
| **Tags**     | `Easy`, `Cryptography`, `picoCTF 2021` |

---

## 📝 Description

> Cryptography can be easy, do you know what ROT13 is?
>
> Download: [values.txt](values.txt)

---

## 🧩 Challenge Overview

We're given a file called `values.txt` containing what appears to be an encoded string. The challenge description directly hints at **ROT13** — a simple letter substitution cipher. Our job is to decode the text to recover the flag.

---

## 💡 Background: What is ROT13?

**ROT13** ("rotate by 13 places") is a special case of the Caesar cipher. It replaces each letter with the letter **13 positions** after it in the alphabet.

Since the English alphabet has 26 letters, applying ROT13 **twice** returns you to the original text — meaning the same operation is used for both encryption and decryption.

```
A B C D E F G H I J K L M N O P Q R S T U V W X Y Z
↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓
N O P Q R S T U V W X Y Z A B C D E F G H I J K L M
```

The name "Mod 26" refers to **modular arithmetic** — shifting by 13 in an alphabet of 26 letters means `(position + 13) mod 26`.

---

## 🧰 Tools Used

- **[CyberChef](https://gchq.github.io/CyberChef/)** — an online tool for encoding/decoding, often called "The Cyber Swiss Army Knife"

---

## 🚶 Walkthrough

### Step 1: Examine the Encoded Text

Open `values.txt`. It contains:

```
cvpbPGS{arkg_gvzr_V'yy_gel_2_ebhaqf_bs_ebg13_45559noq}
```

This looks like a flag format — you can see the structure `cvpbPGS{...}` which resembles `picoCTF{...}` but with letters shifted around. This confirms it's a simple substitution cipher.

### Step 2: Decode with CyberChef

1. Go to [CyberChef](https://gchq.github.io/CyberChef/)
2. Search for **"ROT13"** in the Operations panel on the left
3. Drag the **ROT13** operation into the Recipe area
4. Paste the encoded text into the **Input** box:
   ```
cvpbPGS{arkg_gvzr_V'yy_gel_2_ebhaqf_bs_ebg13_45559noq}
   ```
5. The **Output** box instantly shows the decoded flag!

### Step 3: Get the Flag 🏁

After applying ROT13:

```
picoCTF{next_time_I'll_try_2_rounds_of_rot13_45559abd}
```

---

## 🔑 Key Takeaways

1. **ROT13 is its own inverse.** Applying it once encodes; applying it again decodes. This is because 13 + 13 = 26 (the full alphabet).
2. **"Mod 26" is the hint.** The challenge title tells you the cipher operates in modulo 26 — which is exactly how ROT13 works.
3. **CyberChef is your best friend** for quick encoding/decoding tasks in CTFs. Bookmark it!
4. **Numbers and special characters are not affected** by ROT13 — only alphabetic letters are rotated.

---

## 🏷️ Flag

```
picoCTF{next_time_I'll_try_2_rounds_of_rot13_45559abd}
```
---

[← Previous: The Numbers](../The_Numbers/writeup.md) | [🏠 Home](../../README.md) | [→ Next: 13](../13/writeup.md)
