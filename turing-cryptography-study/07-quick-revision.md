# Quick Revision

*Optional — not counted in the core 15 minutes.*

## 20 essential definitions

1. **Bletchley Park** — the UK's wartime codebreaking HQ, home to GC&CS.
2. **GC&CS (Government Code and Cypher School)** — Britain's codebreaking agency, ancestor of GCHQ.
3. **Enigma** — the German machine used to encrypt military messages.
4. **Bombe** — Allied electromechanical machine that eliminated impossible Enigma settings.
5. **Colossus** — Allied electronic machine used to break the separate Lorenz cipher.
6. **Crib** — a guessed/known fragment of plaintext used to attack a cipher.
7. **Ultra** — the codename for intelligence derived from Allied codebreaking.
8. **Turing Machine** — an abstract mathematical model of computation (1936).
9. **Computability** — whether a problem can be solved by some algorithm in finite steps.
10. **Universal Turing Machine** — a machine that can simulate any other Turing Machine given its description.
11. **Church–Turing thesis** — the claim that "computable by an algorithm" = "computable by a Turing Machine."
12. **Halting Problem** — the proven impossibility of a general algorithm that decides if any program halts.
13. **Turing Test (imitation game)** — a behavioral test of whether a machine's conversation is indistinguishable from a human's.
14. **Cryptography** — the practice of building secure communication/storage systems.
15. **Cryptanalysis** — the practice of breaking those systems without the key.
16. **Symmetric encryption** — same key for encryption and decryption (e.g., AES).
17. **Asymmetric (public-key) encryption** — a public/private key pair (e.g., RSA).
18. **Kerckhoffs's principle** — security should depend only on the secrecy of the key, never the algorithm.
19. **Hash function** — a one-way function mapping data to a fixed-size digest.
20. **MAC / HMAC / Digital signature** — mechanisms that prove a message wasn't tampered with and (for signatures) who sent it.

## Diagram: how the pieces relate

```text
Turing Machine --> Computability --> Modern Computers
Bletchley Park --> Codebreaking   --> Modern Cryptanalysis
Turing Test    --> AI             --> Modern AI Systems
Classical Crypto --> Modern Encryption --> Secure Communication
         (four related but separate threads — see Part 6)
```

## Comparison: Turing Machine vs. Bombe vs. Enigma vs. Turing Test

| | Turing Machine | Enigma | Bombe | Turing Test |
|---|---|---|---|---|
| **What it is** | Abstract theoretical model | Physical encryption device | Physical codebreaking device | A behavioral thought experiment / evaluation method |
| **Real or theoretical** | Theoretical (never physically built as-is) | Real, built and used | Real, built and used | A procedure, not a machine |
| **Purpose** | Define what "computable" means | Scramble messages | Eliminate impossible Enigma settings | Judge if machine conversation seems human |
| **Year** | 1936 | Commercial design from 1920s, military use through WWII | ~1939–40 (British version) | 1950 |
| **Field** | Theoretical computer science | Cryptography (encryption) | Cryptanalysis | Philosophy of mind / AI |

## Comparison: encryption vs. compression vs. hashing vs. MACs vs. digital signatures

| | Reversible? | Needs a secret key? | Main goal | Example |
|---|---|---|---|---|
| **Encryption** | Yes, with the key | Yes | Confidentiality | AES, RSA |
| **Compression** | Yes, always (lossless) or approximately (lossy) | No | Reduce size | Huffman coding, ZIP |
| **Hashing** | No (one-way) | No | Fingerprint / detect changes | SHA-256 |
| **MAC / HMAC** | No | Yes (shared key) | Integrity + authenticity between two parties | HMAC-SHA256 |
| **Digital signature** | No (verify-only, asymmetric) | Yes (private key to sign) | Integrity + authenticity + non-repudiation | RSA/ECDSA signatures |

## Five key takeaways

1. **Breaking Enigma was a team effort across nations and years** — Polish mathematicians broke it first, and Turing's Bombe built on that work alongside Welchman's critical improvements.
2. **The Turing Machine isn't a real device — it's the theoretical yardstick that defines what any computer, no matter how advanced, can ever compute.**
3. **Passing the Turing Test is evidence of convincing behavior, not proof of understanding, reasoning, or consciousness.**
4. **Encryption, compression, and authentication solve three different problems** (secrecy, size, integrity/origin) — none of them substitutes for the others, and real systems combine them deliberately, compression before encryption.
5. **Modern cryptography assumes the algorithm is public** (Kerckhoffs's principle) — security lives entirely in the secrecy of the key, unlike the partial design-secrecy Enigma relied on.

---
← [Part 6: Connecting the Concepts](06-connecting-the-concepts.md) | Next → [Knowledge Check (optional)](08-knowledge-check.md)
