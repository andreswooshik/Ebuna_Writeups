# 🤝 Contributing Guidelines

> How to add new writeups or improve existing ones in this repository.

[🏠 Home](../README.md) | [🔓 CryptoHack](README.md)

---

## 📋 How to Contribute

This is a personal learning repository, but feedback, corrections, and suggestions are welcome.

### Reporting Errors

If you spot a mistake (wrong flag, incorrect explanation, broken code), please [open an issue](https://github.com/andreswooshik/Ebuna_Writeups/issues) with:

- The file path
- What is incorrect
- What the correct information is (if known)

### Suggesting Improvements

Got a better explanation, a cleaner code solution, or a useful resource to add? Open an issue describing the improvement.

---

## 📝 Writing a New Writeup

### 1. Use the Template

Copy [TEMPLATE.md](TEMPLATE.md) as your starting point. It has all the required sections pre-filled with placeholders.

### 2. File Naming

- Use the official challenge name with underscores replacing spaces
- Match the capitalization of the official challenge name
- Examples: `Great_Snakes.md`, `Bytes_and_Big_Integers.md`

### 3. Required Sections

Every writeup must include:

- [ ] Challenge info table (platform, category, difficulty)
- [ ] Challenge description (original text/ciphertext)
- [ ] Concept explanation (suitable for beginners)
- [ ] Step-by-step solution
- [ ] Working solution code with comments
- [ ] Flag in a code block
- [ ] Navigation links (previous / home / next)

### 4. Code Quality

All Python code in writeups should:

- Run correctly with Python 3.8+
- Use meaningful variable names
- Include inline comments explaining non-obvious steps
- Handle the specific challenge input (not just pseudocode)

```python
# Good ✅
ciphertext = bytes.fromhex("0e0b213f...")  # hex-encoded ciphertext from challenge
known = b"crypto{"                          # known flag prefix
key_fragment = bytes(c ^ p for c, p in zip(ciphertext, known))  # KPA

# Bad ❌
ct = bytes.fromhex("...")
k = b"..."
r = bytes(c ^ p for c, p in zip(ct, k))
```

### 5. Formatting Standards

- Use **consistent emoji** headings: 📊 for info, 🧠 for concepts, 🔍 for approach, 💻 for code, ✅ for flag
- Use **fenced code blocks** with language tags (` ```python `, ` ```bash `)
- Use **tables** for structured data (challenge properties, step-by-step tables)
- Keep lines under ~120 characters where possible

### 6. Navigation

Add navigation links at the bottom of every file:

```markdown
[← Previous Challenge](PreviousChallenge.md) | [🏠 Home](../README.md) | [Next Challenge →](NextChallenge.md)
```

Update [CryptoHack/README.md](README.md) to include your new writeup in the challenge map.

---

## ✅ Writeup Checklist

Before adding a new writeup, verify:

- [ ] All code examples tested and produce the correct flag
- [ ] All internal links (navigation, cross-references) work correctly
- [ ] Challenge description accurately reflects the original challenge
- [ ] Concept explanation is accurate and beginner-friendly
- [ ] Flag is correct and in a code block
- [ ] Navigation links point to the right files
- [ ] Challenge map in README.md updated

---

[🏠 Home](../README.md) | [🔓 CryptoHack](README.md) | [📝 Template](TEMPLATE.md)
