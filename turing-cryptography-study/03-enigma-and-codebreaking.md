# Part 3: Enigma and Codebreaking

*Target time: 2 minutes*

**WHAT is cryptanalysis?** Cryptanalysis is the study of breaking encryption schemes — recovering plaintext or a secret key *without* already having the key. It's the mirror image of cryptography (which *builds* secure schemes — see [Part 5](05-cryptography-fundamentals.md)).

## How Enigma encrypted messages

**HOW:** Enigma was an electromechanical machine with these parts:

| Part | Role |
|---|---|
| **Rotors** (3–4, depending on model) | Each rotor is internally wired to substitute one letter for another; after every keystroke, at least one rotor steps forward (like an odometer), so the substitution changes with every letter typed |
| **Reflector** | Sends the electrical signal back through the rotors a second time, which conveniently makes encryption its own inverse — if `A` encrypts to `B` under a setting, `B` encrypts to `A` under that same setting |
| **Plugboard (Steckerbrett)** | Manually swaps pairs of letters before/after the rotors, multiplying the number of possible configurations enormously |
| **Key / daily settings** | Rotor order, rotor starting positions, and plugboard pairs — distributed in advance via codebooks and changed daily |

Because the substitution changes with every keystroke, Enigma is not a simple substitution cipher — frequency analysis (counting which letters appear most, the classic technique against simple ciphers) doesn't directly work.

**WHY it was hard to break:** The number of possible rotor/plugboard configurations ran into the hundreds of quintillions for military variants. Settings also changed daily, so even a fully broken setting was useless the next day — the whole process had to repeat.

## Diagram: encryption vs. cryptanalysis

```text
   Plaintext  --[ Enigma + daily key ]-->  Ciphertext
   (intercepted ciphertext)
   Ciphertext --[ Cryptanalysis: cribs + Bombe + human reasoning ]--> Recovered plaintext
```

## Cribs and the Bombe

**WHAT is a crib?** A **crib** is a guessed or known fragment of plaintext believed to correspond to part of an intercepted ciphertext — for example, predictable message headers, routine weather reports, or stock phrases like "nothing to report." Cribs came from operational knowledge, captured documents, and operator habits, not from breaking the cipher itself.

**HOW the Bombe helped:** The Bombe didn't decrypt messages directly. Given a crib, it rapidly tested enormous numbers of possible rotor settings and used logical contradictions (exploiting the fact Enigma never encrypts a letter to itself, among other properties) to **eliminate impossible settings**, electromechanically, far faster than by hand. This narrowed millions of candidates down to a short list that human cryptanalysts then verified manually on replica Enigma machines.

**Human + machine, working together:**

1. Cryptanalysts and linguists formulated **cribs** from intercepted traffic and operational intelligence.
2. Engineers translated a crib into a "menu" wired into the **Bombe**.
3. The Bombe mechanically narrowed down candidate settings.
4. Humans verified the surviving candidates and decrypted the full message by hand.
5. Decrypted intelligence (codenamed **Ultra**) was passed on, carefully disguised so its source was never exposed to the enemy.

Neither side of this — pure machine brute force, or pure human reasoning — could have succeeded alone at wartime scale and speed.

## Modern cryptanalysis vs. historical Enigma-breaking

| | Enigma-era cryptanalysis | Modern cryptanalysis |
|---|---|---|
| Target | A mechanical cipher with exploitable operational habits and design quirks | Mathematically rigorous, publicly published algorithms (AES, RSA) designed to resist known-plaintext attacks |
| Method | Known/guessed plaintext (cribs) + electromechanical elimination + manual verification | Mathematical structure analysis, side-channel attacks, massive brute-force search (GPUs/ASICs), formal security proofs |
| Secrecy assumption | Partial reliance on the enemy not knowing machine internals | Assumes the attacker **knows the algorithm completely** — only the key is secret (see Kerckhoffs's principle, [Part 5](05-cryptography-fundamentals.md)) |
| Scale | Dozens of Bombes, hundreds of staff | Global computing infrastructure, automated tooling |

**MODERN CONNECTION:** The core idea behind a crib — "exploit any predictable structure or operational mistake rather than brute-forcing everything" — still underlies real-world attacks today, from guessing weak passwords based on patterns, to exploiting implementation bugs rather than attacking the math of AES directly.

---
← [Part 2: Turing Machine](02-turing-machine.md) | Next → [Part 4: Turing Test & AI](04-turing-test-and-ai.md)
