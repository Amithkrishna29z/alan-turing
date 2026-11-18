# Knowledge Check

*Optional — not counted in the core 15 minutes. Try answering before checking the answer key.*

## Part A — 10 conceptual questions

1. What was the Government Code and Cypher School, and what is its modern-day descendant?
2. Why couldn't Enigma be broken using simple frequency analysis?
3. What is a "crib," and where did cribs come from?
4. What is the Universal Turing Machine, and why does it matter for real computers?
5. What does the Halting Problem actually prove?
6. Why doesn't passing the Turing Test prove a machine is "intelligent"?
7. What is Kerckhoffs's principle, and why does modern cryptography follow it?
8. Why is data normally compressed *before* it's encrypted, rather than after?
9. What does a MAC prove that a plain hash does not?
10. What extra guarantee does a digital signature provide that a MAC does not?

## Part B — 5 scenario-based questions

1. You intercept an encrypted enemy message and happen to know it always starts with the same greeting phrase. What cryptographic/historical concept does this let you exploit, and what WWII-era machine was built to exploit it at scale?
2. A chatbot convinces a human judge in a text conversation that it's human. A colleague says "this proves the chatbot is truly intelligent." What's wrong with that conclusion?
3. A developer compresses a secret API key together with attacker-supplied text before encrypting both, and notices the ciphertext length changes depending on what the attacker sends. Why is this a security risk?
4. A system stores user passwords by encrypting them with a secret key so they can be decrypted later for "convenience." What's wrong with this design, and what should it do instead?
5. Someone claims "if we just keep our encryption algorithm secret, we don't need a very long key." Which principle does this violate, and why is it bad practice?

## Part C — 5 true/false misconception checks

1. Alan Turing single-handedly broke the Enigma cipher.
2. A Turing Machine is a real physical machine Turing built during WWII.
3. Encryption and encoding (like Base64) are the same thing.
4. A message that's encrypted is automatically also protected against undetected tampering.
5. Colossus was built to break the Enigma cipher.

---

## Answer Key

### Part A
1. **It was Britain's WWII codebreaking agency, based at Bletchley Park; its modern descendant is GCHQ.**
2. **Because Enigma's substitution changed with every single keystroke** (via stepping rotors), so it wasn't a fixed one-to-one letter substitution — frequency analysis assumes a stable mapping, which Enigma didn't have.
3. **A crib is a guessed or known fragment of plaintext believed to match part of a ciphertext.** Cribs came from operational intelligence, predictable message formats, and operator habits — not from mathematically breaking the cipher itself.
4. **It's a single Turing Machine that can simulate any other Turing Machine given a description of it plus input.** It's the theoretical basis for the stored-program computer — one general-purpose machine that runs whatever program you load into it, instead of needing a different machine built per task.
5. **It proves no general algorithm can decide, for every possible program and input, whether that program will eventually halt.** This is a mathematical impossibility, not a current technical limitation.
6. **Because the test only measures conversational behavior, not internal reasoning, understanding, or consciousness.** A system can produce convincing text without any of those being present.
7. **Kerckhoffs's principle: a cryptographic system should stay secure even if everything about it except the key is public.** Modern crypto follows it because secret algorithm designs eventually leak or get reverse-engineered, and public algorithms get far more scrutiny to find weaknesses before attackers do.
8. **Because compression relies on finding redundancy/patterns in data, and well-encrypted data looks like random noise with no patterns left** — compressing after encrypting gains almost nothing, so compression has to happen first, while the redundancy is still there.
9. **A MAC proves the message wasn't modified AND that it was produced by someone holding a specific shared secret key.** A plain hash proves only that data matches a digest — anyone can compute a hash, so it says nothing about who produced the data.
10. **Non-repudiation.** Because only the signer holds the private key, they can't credibly deny having signed — with a MAC, either party sharing the key could have produced it, so there's no way to prove which one did.

### Part B
1. **This is exactly what a crib is** — a predictable fragment of known plaintext. The Allied **Bombe** was built to exploit cribs at mechanical scale, rapidly eliminating Enigma settings inconsistent with the guessed plaintext.
2. **Passing the test shows convincing conversational behavior, not proven intelligence, reasoning, understanding, or consciousness** — those are separate properties the Turing Test was never designed to measure.
3. **This is a real, known class of attack (similar to real-world TLS compression attacks like CRIME/BREACH).** Because compression ratio depends on how much the secret overlaps with attacker-chosen content, the resulting ciphertext *length* can leak information about the secret — the fix is to never compress secrets together with attacker-controlled data before encryption.
4. **Passwords should never be reversible.** They should be run through a slow, salted *hash* function (e.g., bcrypt/Argon2) so the original password is never stored and can't be recovered even if the database leaks — "needing to decrypt it later" is itself the design flaw.
5. **This violates Kerckhoffs's principle**, which says security should rest entirely on the secrecy of the key, not the algorithm. Relying on a secret algorithm is fragile — once the design leaks (which history shows eventually happens), there is no fallback security margin if the key itself is also weak.

### Part C
1. **False.** Breaking Enigma was a multi-year, multinational effort — Polish mathematicians (Rejewski and colleagues) broke it first in the 1930s, and Gordon Welchman's improvements were critical to the British Bombe's effectiveness alongside Turing's.
2. **False.** The Turing Machine is a purely abstract, theoretical model described in a 1936 paper — no one has ever built one literally (its tape would need to be infinite), though it's foundational to the design of all real computers.
3. **False.** Encoding (like Base64) reformats data for compatibility and is trivially reversible by anyone — it uses no key and provides no secrecy. Encryption requires a secret key and is specifically designed to resist unauthorized reversal.
4. **False.** Encryption alone protects confidentiality but does **not** by itself detect tampering — that requires a separate integrity mechanism such as a MAC, HMAC, or digital signature.
5. **False.** Colossus was built to break the separate **Lorenz cipher** ("Tunny"), used for high-level strategic communications — not Enigma, which the Bombe targeted.

---
← [Quick Revision](07-quick-revision.md) | [Back to README](README.md)
