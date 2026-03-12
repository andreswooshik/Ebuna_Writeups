# 🔐 Ebuna CTF Writeups

> *A personal cryptography learning journal — documenting solutions, breaking down concepts, and building intuition for real-world security.*

[![Platforms](https://img.shields.io/badge/Platforms-CryptoHack%20%7C%20TryHackMe%20%7C%20picoCTF-blue?style=flat-square)](https://cryptohack.org)
[![Language](https://img.shields.io/badge/Language-Python%203-yellow?style=flat-square&logo=python)](https://python.org)
[![Tools](https://img.shields.io/badge/Tools-CyberChef%20%7C%20pwntools%20%7C%20PyCryptodome-green?style=flat-square)](https://gchq.github.io/CyberChef/)

---

## 📖 About This Repository

This repository contains personal writeups for cryptography challenges across multiple CTF platforms. Each writeup goes beyond just the solution — it breaks down the underlying math, explains *why* the vulnerability exists, and connects concepts to real-world security.

**Author:** andreralf25 | **Started:** 2026

---

## 📊 Progress Tracker

### 🔓 CryptoHack

| Track | Progress | Status |
|-------|----------|--------|
| Introduction | `██████████` 10/10 | ✅ Completed |
| Modular Arithmetic | `░░░░░░░░░░` 0/10 | ⬜ Planned |
| Symmetric Cryptography | `░░░░░░░░░░` 0/11 | ⬜ Planned |
| Public-Key Cryptography | `░░░░░░░░░░` 0/14 | ⬜ Planned |
| Elliptic Curves | `░░░░░░░░░░` 0/18 | ⬜ Planned |

### 🧠 TryHackMe

| Room | Status | Points |
|------|--------|--------|
| Cryptography Basics | ✅ Completed | 72 pts |
| W1seGuy | ✅ Completed | 60 pts |

### 🏆 picoCTF

| Challenge | Year | Status |
|-----------|------|--------|
| The Numbers | 2019 | ✅ Completed |
| 13 (ROT13) | 2019 | ✅ Completed |
| Mod 26 | 2021 | ✅ Completed |
| EVEN RSA CAN BE BROKEN | 2025 | ✅ Completed |

---

## 🗂️ Quick Navigation

### 🔓 CryptoHack Writeups
→ [CryptoHack Overview & Challenge Map](CryptoHack/README.md)

| # | Challenge | Category | Difficulty |
|---|-----------|----------|-----------|
| 1 | [You Either Know, XOR You Don't](CryptoHack/You_Either_Know_XOR_You_Dont.md) | XOR / Known-Plaintext | 🟢 Easy |
| 2 | [Great Snakes](CryptoHack/Great_Snakes.md) | Python Basics | 🟢 Easy |
| 3 | [ASCII](CryptoHack/ASCII.md) | Encoding | 🟢 Easy |
| 4 | [HEX](CryptoHack/HEX.md) | Encoding | 🟢 Easy |
| 5 | [Base64](CryptoHack/Base64.md) | Encoding | 🟢 Easy |
| 6 | [Bytes and Big Integers](CryptoHack/Bytes_and_Big_Integers.md) | Encoding | 🟢 Easy |
| 7 | [XOR Starter](CryptoHack/XOR_Starter.md) | XOR | 🟢 Easy |
| 8 | [XOR Properties](CryptoHack/XOR_Chain.md) | XOR | 🟢 Easy |

### 🧠 TryHackMe Writeups

| Room | Topics | Difficulty |
|------|--------|------------|
| [W1seGuy](TryHackMe/W1seGuy.md) | Caesar Cipher, XOR Known-Plaintext | 🟢 Easy |

### 🏆 picoCTF Writeups

| Challenge | Topic | Year |
|-----------|-------|------|
| [The Numbers](picoCTF/The_Numbers/writeup.md) | A1Z26 Cipher | 2019 |
| [13](picoCTF/13/writeup.md) | ROT13 | 2019 |
| [Mod 26](picoCTF/Mod_26/writeup.md) | ROT13 / Modular Arithmetic | 2021 |
| [EVEN RSA CAN BE BROKEN](picoCTF/EVEN_RSA_CAN_BE_BROKEN/writeup.md) | RSA Factoring | 2025 |

---

## 🎓 Recommended Learning Path

```
Beginner ─────────────────────────────────────────────────────►
│
├─ 1. picoCTF: The Numbers        (A1Z26 — no coding needed)
├─ 2. picoCTF: Mod 26             (ROT13 — CyberChef)
├─ 3. picoCTF: 13                 (ROT13 — CyberChef)
├─ 4. TryHackMe: W1seGuy Task 1   (Caesar Cipher brute force)
├─ 5. TryHackMe: W1seGuy Task 2   (XOR known-plaintext attack)
└─ 6. CryptoHack: XOR You Don't   (XOR KPA + key recovery)

Intermediate ─────────────────────────────────────────────────►
│
├─ 7. CryptoHack: Modular Arithmetic track
└─ 8. picoCTF: EVEN RSA CAN BE BROKEN (RSA factoring)
```

---

## 🛠️ Skills Covered

| Skill | Challenges |
|-------|-----------|
| ROT13 / Caesar Cipher | picoCTF 13, Mod 26, W1seGuy Task 1 |
| XOR Cipher | W1seGuy Task 2, CryptoHack XOR |
| Known-Plaintext Attack | W1seGuy Task 2, CryptoHack XOR |
| A1Z26 Encoding | picoCTF The Numbers |
| RSA Basics | picoCTF EVEN RSA |
| Python Scripting | W1seGuy, CryptoHack XOR, EVEN RSA, The Numbers |
| CyberChef | picoCTF Mod 26, picoCTF 13 |

---

## 🏆 Achievements

<details>
<summary><b>🔐 Cryptography Basics — TryHackMe (72 pts)</b></summary>

| Completed Tasks | Points Earned | Streak |
|:-:|:-:|:-:|
| 7 | 72 | 1 |

> *andreralf25 completed another room on their cyber security journey.*
</details>

<details>
<summary><b>🧠 W1seGuy — TryHackMe (60 pts)</b></summary>

| Completed Tasks | Points Earned | Streak |
|:-:|:-:|:-:|
| 2 | 60 | 1 |

> *andreralf25 completed another room on their cyber security journey.*
</details>

<details>
<summary><b>🔑 You Either Know, XOR You Don't — CryptoHack (30 pts)</b></summary>

| Completed Tasks | Points Earned | Streak |
|:-:|:-:|:-:|
| 1 | 30 | 1 |

> *andreralf25 completed another challenge on their cyber security journey.*
</details>

---

## 🗺️ 3-Week Roadmap

| Week | Track | Time | Topics |
|------|-------|------|--------|
| 1 | Introduction + Modular Arithmetic | ~6 days | Encoding, XOR, modular inverse |
| 2 | Symmetric Cryptography | ~3 days | AES, block ciphers, padding oracles |
| 3 | Public-Key + Elliptic Curves | ~10 days | RSA, Diffie-Hellman, ECDH, ECDSA |
| — | **Total** | **~3 Weeks (2–3 hrs/day)** | — |

---

## 🔗 Platforms

| Platform | Link | Description |
|----------|------|-------------|
| CryptoHack | [cryptohack.org](https://cryptohack.org) | Structured crypto learning with free challenges |
| TryHackMe | [tryhackme.com](https://tryhackme.com) | Guided rooms with beginner-friendly paths |
| picoCTF | [picoctf.org](https://picoctf.org) | Archive of real CTF challenges, great for beginners |

---

## 🤝 Contributing

Want to suggest improvements or spot an error? See [CryptoHack/CONTRIBUTING.md](CryptoHack/CONTRIBUTING.md) for guidelines.

---

## 🛠️ Tech Stack

- **Python 3** — primary scripting language
- **CyberChef** — quick encoding/decoding in the browser
- **pwntools** — CTF scripting framework
- **PyCryptodome** — cryptographic primitives (`pip install pycryptodome`)
- **netcat** — server interaction (`nc`)
