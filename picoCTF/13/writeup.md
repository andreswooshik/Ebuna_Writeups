# 13

| Detail       | Value                          |
|--------------|--------------------------------|
| **CTF**      | picoCTF 2019                   |
| **Category** | Cryptography                   |
| **Difficulty** | Easy                         |
| **Author**   | Alex Fulton / Daniel Tunitis   |
| **Solves**   | 104,183 (88% liked)            |
| **Tags**     | `Easy`, `Cryptography`, `picoCTF 2019` |

---

## 📝 Description

> Cryptography can be easy, do you know what ROT13 is?
>
> `cvpbPGS{abg_gbb_onq_bs_n_ceboyrz}`

---

## 🧩 Challenge Overview

We're given an encoded string directly in the challenge description. The challenge name is **"13"** and the description asks if we know what **ROT13** is — so this is about as straightforward as it gets. Apply ROT13 to the ciphertext to get the flag.

---

## 💡 Background: What is ROT13?

**ROT13** ("rotate by 13 places") is a simple letter substitution cipher that shifts each letter 13 positions forward in the alphabet.

```
A B C D E F G H I J K L M N O P Q R S T U V W X Y Z
↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓
N O P Q R S T U V W X Y Z A B C D E F G H I J K L M
```

Since the alphabet has 26 letters and 13 is exactly half, **ROT13 is its own inverse** — applying it twice gives you back the original text. The challenge name "13" is the hint itself!

---

## 🧰 Tools Used

- **[CyberChef](https://gchq.github.io/CyberChef/)** — online encoding/decoding tool

---

## 🚶 Walkthrough

### Step 1: Identify the Ciphertext

The encoded string is given right in the description:

```
cvpbPGS{abg_gbb_onq_bs_n_ceboyrz}
```

Notice the structure — `cvpbPGS{...}` looks like it could be `picoCTF{...}` after decoding. Let's verify:

| Encoded | c | v | p | b | P | G | S |
|---------|---|---|---|---|---|---|---|
| ROT13   | p | i | c | o | C | T | F |

That's `picoCTF` — we're on the right track!

### Step 2: Decode with CyberChef

1. Go to [CyberChef](https://gchq.github.io/CyberChef/)
2. Search for **"ROT13"** in the Operations panel
3. Drag the **ROT13** operation into the Recipe area
4. Paste the ciphertext into the **Input** box:
   ```
   cvpbPGS{abg_gbb_onq_bs_n_ceboyrz}
   ```
5. The **Output** box instantly shows the decoded flag!

### Step 3: Manual Decode (for learning)

Let's walk through the full decode letter by letter:

| Encoded | Decoded |
|---------|---------|
| c → p | v → i | p → c | b → o |
| P → C | G → T | S → F |
| { → { |
| a → n | b → o | g → t |
| _ → _ |
| g → t | b → o | b → o |
| _ → _ |
| o → b | n → a | q → d |
| _ → _ |
| b → o | s → f |
| _ → _ |
| n → a |
| _ → _ |
| c → p | e → r | b → o | o → b | y → l | r → e | z → m |
| } → } |

Result: **picoCTF{not_too_bad_of_a_problem}**

### Step 4: Get the Flag 🏁

```
picoCTF{not_too_bad_of_a_problem}
```

---

## 🔑 Key Takeaways

1. **The challenge name IS the hint.** "13" → ROT13. Always pay attention to challenge titles in CTFs.
2. **ROT13 is self-inverse.** Encoding and decoding use the exact same operation — just rotate by 13.
3. **Non-alphabetic characters are untouched.** Curly braces `{}`, underscores `_`, and numbers pass through ROT13 unchanged.
4. **This is a sister challenge to "Mod 26"** — both are picoCTF ROT13 challenges. "Mod 26" refers to the same concept since `(position + 13) mod 26` is how ROT13 works mathematically.

---

## 🏷️ Flag

```
picoCTF{not_too_bad_of_a_problem}
```