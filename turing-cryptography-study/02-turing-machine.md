# Part 2: The Turing Machine — Foundation of Computing

*Target time: 3 minutes*

## 1. What is a Turing Machine?

**WHAT:** A Turing Machine (TM) is not a physical device — it's a simple, abstract mathematical model of computation that Alan Turing described in his 1936 paper *"On Computable Numbers, with an Application to the Entscheidungsproblem."*

**WHY it was invented:** Mathematician David Hilbert had posed the **Entscheidungsproblem** ("decision problem"): is there a general mechanical procedure that can decide whether *any* mathematical statement is true? To answer that, Turing first needed to pin down precisely what "a mechanical procedure" — what we'd now call an **algorithm** — actually means. His machine was that definition. Using it, he proved the answer to Hilbert's question is **no** (see the Halting Problem below).

**HOW it relates to real computers:** No one has ever built a literal Turing Machine — its tape is infinite, which no physical device can be. But it defines the *ceiling* of what's computable at all. Every real computer, no matter how fast or advanced, computes a subset of what a Turing Machine could compute, given enough time and memory.

## 2. Components

| Component | Role |
|---|---|
| **Tape** | An infinitely long strip of cells, each holding one symbol (theoretical — real memory is finite) |
| **Read/write head** | Reads the symbol under it, can write a new symbol, then moves one cell left or right |
| **States** | A finite set of "modes" the machine can be in (think: which page of the rulebook it's on) |
| **Symbols** | The finite alphabet that can appear on the tape (e.g., `0`, `1`, blank) |
| **Transition rules** | "If in state X reading symbol Y → write symbol Z, move left/right, go to state W" |
| **Halting condition** | A special state that, once reached, stops the machine |

```text
   ... [0][1][1][0][_][_] ...     <- the infinite tape
             ^
           [head]                 <- reads/writes one cell, moves L or R
             |
      +-------------+
      |  state: q1  |              <- current "mode," looked up in the
      +-------------+                 transition rule table below
      Rule table (example):
        q0, read 1 -> write 1, move Right, go to q0
        q0, read _ -> write _, move Left,  go to q1
        q1, read 1 -> write 0, move Left,  go to q1
        q1, read 0 -> write 1, move Left,  go to HALT
```

**Simple analogy:** Imagine a person with an infinite notebook and a strict rulebook. They look at the symbol on the current page, consult the rulebook for their current "state of mind," write a new symbol if told to, flip one page left or right, and change their state of mind. They repeat until the rulebook says "stop." That's the entire model — nothing more exotic than that.

## 3. Worked example: incrementing a binary number

**EXAMPLE:** Add 1 to the binary number `1011` (decimal 11) to get `1100` (decimal 12), using only the rules below.

**Rules (plain English):**
1. **State `q0` (scan right to find the end):** move right over `0`s and `1`s until a blank is found; then step back left one cell and switch to state `q1`.
2. **State `q1` (add 1, carrying right-to-left):**
   - If the cell holds `1`, write `0`, move left, **stay in `q1`** (the carry continues).
   - If the cell holds `0`, write `1`, move left, **go to `HALT`** (no more carry needed).

**Step-by-step trace:**

| Step | Position (left→right) | Action | Tape after |
|---|---|---|---|
| 0 | start | tape is `1 0 1 1`, head at leftmost digit, state `q0` | `1011` |
| 1 | scanning right | read `1`,`0`,`1`,`1` in turn, moving right each time (still `q0`) | `1011` |
| 2 | past the end | read blank → step back left, switch to `q1` | `1011` |
| 3 | rightmost `1` | read `1` → write `0`, move left (carry) | `1010` |
| 4 | next `1` | read `1` → write `0`, move left (carry) | `1000` |
| 5 | next `0` | read `0` → write `1`, move left, **HALT** | `1100` |

**Result: `1100`** — exactly 11 + 1 = 12 in binary. Each step followed only the mechanical rule table; no "intelligence" was needed, just strict rule-following — which is the whole point.

## 4. Important concepts

<details>
<summary><strong>Algorithms & computability</strong> — click to expand</summary>

An **algorithm** is a finite, unambiguous sequence of steps that solves a problem. A problem is **computable** if some Turing Machine (equivalently, some algorithm) can solve it for every valid input in a finite number of steps.
</details>

<details>
<summary><strong>Universal Turing Machine</strong> — click to expand</summary>

A **Universal Turing Machine (UTM)** is a single Turing Machine that can read a *description* of any other Turing Machine plus its input, and simulate it. This is the theoretical ancestor of the **stored-program computer**: one general-purpose machine that runs whatever program (data) you feed it, instead of needing a different hand-built machine per task.
</details>

<details>
<summary><strong>Church–Turing thesis</strong> — click to expand</summary>

Independently, Alonzo Church (via lambda calculus) and Alan Turing (via the Turing Machine) arrived at equivalent definitions of "computable." The **Church–Turing thesis** is the (unprovable but universally accepted) claim that *anything* intuitively "computable by an algorithm" can be computed by a Turing Machine — no stronger model of mechanical computation exists.
</details>

<details>
<summary><strong>The Halting Problem</strong> — click to expand</summary>

Turing proved there is **no general algorithm** that can, for every possible program and input, correctly decide whether that program will eventually halt or run forever. This isn't a limitation of current technology — it's a mathematical impossibility, proven by a self-reference/contradiction argument. It's the direct answer to Hilbert's Entscheidungsproblem: no universal decision procedure exists for all mathematical statements either.
</details>

**Turing Machine vs. a modern CPU:**

| | Turing Machine | Modern CPU |
|---|---|---|
| Memory | Infinite tape (theoretical) | Finite RAM/storage (practical) |
| Speed | Irrelevant — only asks "possible or not" | Billions of operations/second |
| I/O, interrupts, multiple cores | Not modeled | Present in real hardware |
| Computational power | Defines the theoretical ceiling | A fast, finite, practical approximation of that ceiling |

**MODERN CONNECTION:** Every programming language and every CPU is, in computational-power terms, equivalent to a Turing Machine (ignoring memory limits) — this is why a "Turing-complete" language can, in principle, compute anything any other Turing-complete language can. The Halting Problem is also why no antivirus or compiler can *perfectly* detect all malicious programs or all infinite loops in general — it's not an engineering gap, it's a proven limit.

---
← [Part 1: Bletchley Park](01-bletchley-park-and-alan-turing.md) | Next → [Part 3: Enigma & Codebreaking](03-enigma-and-codebreaking.md)
