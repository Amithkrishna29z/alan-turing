# Part 5: Cryptography — First Principles

*Target time: 4 minutes*

**WHAT:** Cryptography is the study and practice of techniques for securing information and communication against adversaries.

**WHY:** It solves the problem of letting intended parties exchange or store information over channels (or in places) that untrusted parties can also access — without the untrusted party being able to read, undetectably modify, or convincingly forge it.

**Core vocabulary:**

| Term | Meaning |
|---|---|
| **Plaintext** | The original, readable message |
| **Ciphertext** | The scrambled, encrypted form of the message |
| **Key** | A secret parameter that controls encryption/decryption |
| **Encryption** | Transforming plaintext → ciphertext using a key |
| **Decryption** | Reversing ciphertext → plaintext using a key |
| **Cryptography vs. cryptanalysis** | Building secure systems, vs. breaking them without the key (see [Part 3](03-enigma-and-codebreaking.md)) |
| **Symmetric encryption** | Same key used to encrypt and decrypt (e.g., AES) |
| **Asymmetric encryption** | A mathematically linked key *pair* — public key and private key (e.g., RSA) |

**Four security goals these tools serve (often called the "CIA" triad, plus authenticity):**

| Goal | Question it answers |
|---|---|
| **Confidentiality** | Can only intended parties read this? |
| **Integrity** | Has this been modified since it was sent? |
| **Authenticity** | Did this really come from who it claims to? |
| **Availability** | Is the system usable when it's needed? (a broader security property, not purely cryptographic) |

---

## 1. Encryption and Deception

**WHAT encryption protects:** confidentiality — keeping content unintelligible to anyone without the key.

**HOW it works conceptually:** apply a reversible mathematical transformation, parameterized by a secret key, to plaintext; only someone with the correct key can invert the transformation efficiently.

**EXAMPLE — Caesar cipher (a teaching tool, not real security):** shift every letter forward by a fixed number, e.g., shift 3: `A→D, B→E, ... HELLO → KHOOR`. Trivially broken — only 25 possible shifts, and frequency analysis cracks it in seconds. Useful only to build intuition.

**Modern schemes:**
- **AES** (Advanced Encryption Standard): a symmetric block cipher, operating on 128-bit blocks with 128/192/256-bit keys, using multiple rounds of substitution and permutation. Computationally infeasible to brute-force with current or foreseeable technology.
- **RSA / public-key cryptography**: based on the difficulty of factoring very large numbers. The **public key** can be shared openly (used to encrypt, or verify a signature); the **private key** stays secret (used to decrypt, or sign). This elegantly solves the problem symmetric crypto has — how do two parties agree on a shared secret key in the first place?

**Three distinctions worth being precise about:**

| Compared to encryption | Key difference |
|---|---|
| **Encoding** (e.g., Base64) | Reformats data for compatibility/transmission — no key, trivially reversible by anyone, provides **no** secrecy at all |
| **Steganography** | Hides the *existence* of a message (e.g., concealed in an image's pixel data) rather than making its content unreadable; can be combined with encryption |
| **Deception** | A human/operational concept — misleading an adversary's *beliefs* (e.g., WWII decoy operations feeding false intelligence). This is **not** a standard synonym for encryption: encryption makes content mathematically unreadable; deception manipulates what an adversary believes is true. They solve different problems. |

**Why secrecy shouldn't depend on hiding the algorithm:** This is **Kerckhoffs's principle** — a cryptographic system should remain secure even if everything about it *except the key* is public. Modern algorithms like AES were developed through open, public competitions specifically so the wider cryptographic community could try to break them. Algorithms that rely on secret designs tend to fail once the design leaks or is reverse-engineered (Enigma's design secrecy was only ever a partial, temporary defense).

---

## 2. Data Compression

**WHAT:** Reducing the size of a data representation.

**WHY:** Saves storage space, bandwidth, and transmission time.

| Type | Behavior | Example use |
|---|---|---|
| **Lossless** | Original data reconstructed exactly | ZIP, PNG, text files |
| **Lossy** | Some detail discarded, usually imperceptibly | JPEG, MP3 |

**HOW — two classic techniques:**
- **Huffman coding:** assigns shorter binary codes to frequently occurring symbols and longer codes to rare ones, based on a frequency-ordered binary tree — this is an *optimal* prefix-free code for a known symbol-frequency distribution.
- **Repeated-pattern compression (run-length encoding, dictionary methods):** replaces repeated sequences with shorter references to them.

**EXAMPLE:**
```text
AAAAABBBCC   (10 characters)
  -> 5A3B2C  (6 characters, run-length encoded)
```
This only saves space **because the input has repetition**. A string like `ABCDEFGHIJ` (no repeats) would come out *longer* under the same scheme (`1A1B1C1D...`) — compression helps only when redundancy exists to exploit.

**Why compression is not encryption, and not a security service by itself:** Compressed data is **not secret**. The compression algorithm is public and standard, so anyone who knows it (which is everyone, since it isn't a secret) can decompress the data back to its original form — compression provides no confidentiality, no integrity, and no authenticity on its own.

**Why compress *before* encrypting:** Compression works by finding statistical redundancy/patterns in data. Good encryption output is designed to look like uniform random noise — no patterns left to exploit. So compressing *after* encrypting gains almost nothing; compressing *before* encrypting captures the redundancy while it's still there.

**A real security risk to know about:** If you compress secret data *together with* attacker-influenced input before encrypting it, the **size of the resulting ciphertext can leak information about the secret** — because the compression ratio depends on how much the secret overlaps with the attacker's chosen content. This is exactly what happened in real attacks against TLS compression (e.g., CRIME/BREACH). The general lesson: never compress attacker-controlled data together with a secret before encryption.

**MODERN CONNECTION:** Nearly every file you download, stream, or send over HTTPS has been compressed and encrypted — in that order — somewhere in the pipeline.

---

## 3. Message Authentication

**WHAT:** Verifying that a message is genuine — that it wasn't altered in transit, and (depending on the mechanism) that it actually came from the party it claims to.

**WHY integrity matters:** Encryption alone only hides content — it does **not**, by itself, prevent someone from tampering with the ciphertext in transit. Without a way to detect tampering, a recipient could silently act on altered information (e.g., a changed bank transfer amount or a changed command).

**The building blocks:**

| Mechanism | What it uses | What it proves |
|---|---|---|
| **Cryptographic hash** | A deterministic one-way function mapping any input to a fixed-size digest; a tiny input change drastically changes the output | That data matches a known digest — but **anyone** can compute a hash, so by itself it proves nothing about *who* sent it |
| **MAC (Message Authentication Code)** | A hash-like function combined with a **shared secret key** | That the message is unmodified **and** was produced by someone holding the shared key |
| **HMAC** | A specific, well-analyzed standard way of combining a hash function with a secret key (nested, not naive concatenation) | Same as a MAC, but resistant to certain structural attacks that naive key+hash combinations are vulnerable to |
| **Digital signature** | Asymmetric keys — signer uses a **private** key to sign (usually a hash of the message); anyone can verify with the signer's **public** key | Integrity **and** authenticity **and** non-repudiation (the signer can't credibly deny signing, since only they hold the private key) |

**Two concepts people conflate:**

| | Meaning |
|---|---|
| **Authentication** | Verifying identity/origin — "who sent this?" |
| **Authorization** | Determining what an *authenticated* party is allowed to do — "what can they access?" |

| | Meaning |
|---|---|
| **Integrity** | Was this changed? |
| **Confidentiality** | Can unauthorized parties read this? |

These are independent: a message can be signed-but-unencrypted (public can read it, but tampering is detectable), or encrypted-but-unauthenticated (secret, but tampering might go unnoticed) — real systems need both, deliberately combined.

**EXAMPLE — Alice sends Bob a message with a MAC:**

```text
Alice: message + MAC(shared_key, message)  ----> Bob
Bob:   recomputes MAC(shared_key, received_message)
       if it matches the MAC Alice sent -> message is intact
                                            and came from someone with the shared key
       if it doesn't match             -> reject: tampered, or wrong key
```

If an attacker, Mallory, intercepts and modifies the message in transit, Bob's recomputed MAC won't match the one Alice attached — tampering is detected.

**MACs vs. digital signatures — different guarantees:** A MAC proves integrity and authenticity *between the two parties who share the secret key* — but since both parties hold the same key, either one could have produced a given MAC, so a MAC gives **no non-repudiation**. A digital signature additionally gives non-repudiation, because only the private-key holder could have produced a valid signature, and any third party can verify it using the public key alone.

---

## 4. Real-World Applications

| Technology | What's actually used |
|---|---|
| **HTTPS / TLS** | Asymmetric crypto for key exchange and certificates, symmetric encryption (e.g., AES) for the bulk data, plus MACs/authenticated encryption for integrity — all combined |
| **Banking apps** | Encryption in transit/at rest, strong multi-factor authentication, signed transactions |
| **Password storage** | **Not** encrypted — passwords are run through a slow, salted *hash* function (e.g., bcrypt, scrypt, Argon2) so the plaintext password is never stored and can't easily be reversed even if the database leaks |
| **Secure messaging** (e.g., Signal) | End-to-end encryption, often with per-message keys (forward secrecy), authenticated with MACs/signatures |
| **Software updates** | Digital signatures let a device verify an update genuinely came from the vendor and wasn't tampered with before installing it |
| **API authentication** | Request-signing (HMAC) or signed tokens (e.g., JWTs) prove a caller's identity and that a request wasn't altered |

**One-line distinctions to keep straight:**
- **Encryption** — reversible, needs a key, provides confidentiality.
- **Hashing** — one-way, no key required by itself, used for integrity checks and (combined with salting) password storage.
- **Authentication** (MAC/password/MFA) — proves identity or origin.
- **Digital signatures** — asymmetric-key-based; add authenticity, integrity, *and* non-repudiation.

---
← [Part 4: Turing Test & AI](04-turing-test-and-ai.md) | Next → [Part 6: Connecting Everything](06-connecting-the-concepts.md)
