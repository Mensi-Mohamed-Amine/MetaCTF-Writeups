Here is a clean and concise **writeup** for the cryptography challenge, without emojis:

---

## Cryptography Challenge Writeup: Decrypting the Rotating Cipher Wheel

### Challenge Description

My friend wanted to communicate using a secret cipher.
He gave me the following ciphertext:

```
RKPUYPFCIAKKJMYZZJT
```

Along with a cipher wheel, where:

- The **outer wheel** contains the characters: `A-Z{}`
- The **inner wheel** rotates, changing the substitution mapping each time

---

### Understanding the Cipher

The provided image shows a **substitution cipher wheel** with two concentric rings:

- The **outer ring** remains fixed.
- The **inner ring** rotates one position with each decryption attempt.

The substitution follows this mapping:

```
Outer (plaintext) -> Inner (ciphertext)
```

Since the inner wheel rotates step by step, this mimics a **rotating Caesar cipher**, or a **wheel-based monoalphabetic cipher**.

---

### Decryption Approach

To decrypt the message, we simulate the rotating inner wheel. For each decryption attempt:

1. Align the current `inner_wheel` with the fixed `outer_wheel`.
2. Substitute each character in the ciphertext using the current mapping.
3. Rotate the inner wheel by one position for the next attempt.
4. Stop when the output contains a recognizable flag pattern (e.g., `METACTF{`).

---

### Python Solution

```python
from collections import deque

def decrypt(ciphertext, outer_wheel, inner_wheel):
    decrypted_text = ""
    for char in ciphertext:
        if char in outer_wheel:
            index = outer_wheel.index(char)
            decrypted_text += inner_wheel[index]
        else:
            decrypted_text += char
    return decrypted_text

def find_flag(ciphertext, outer_wheel, inner_wheel):
    for _ in range(len(outer_wheel)):
        decrypted_text = decrypt(ciphertext, outer_wheel, inner_wheel)
        if "METACTF{" in decrypted_text:
            return decrypted_text
        # Rotate the inner wheel for next attempt
        items = deque(inner_wheel)
        items.rotate(1)
        inner_wheel = ''.join(items)

# Cipher wheels
outer_wheel = "ABCDEFGHIJKLMNOPQRSTUVWXYZ{}"
inner_wheel = "QNFUVWLEZYXPTKMR}ABJICOSDHG{"

# Encrypted message
ciphertext = "RKPUYPFCIAKKJMYZZJT"

# Decrypt
print(find_flag(ciphertext, outer_wheel, inner_wheel))
```

---

### Solution

Running the script outputs:

```
METACTF{WHEELYCOOL}
```

---

### Conclusion

This challenge used a dynamic substitution cipher, where the **inner wheel rotates** with each decryption attempt. The flag was successfully revealed by simulating this rotation and testing for a known prefix.

**Flag:** `METACTF{WHEELYCOOL}`

---

Let me know if you want a version formatted for a PDF or a CTF platform submission.
