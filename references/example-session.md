# Example session — Standard mode, 5 questions

A real session captured during study-buddy's first dogfood test. Subject: introductory C++ (assignment, expressions, variable mutation, identifier rules). Source: a learner's own canonical module note. Gap since last study session: 7 days. Names redacted; structure and grader moves preserved verbatim.

This file is for **reading**, not running. It exists so the skill has a concrete reference for what good output looks like.

---

## Quiz Attempts

### YYYY-MM-DD — Study Buddy standard session (gap: 7 days)

**Calibrated to:** Issues & Learnings tags — `#vocabulary` (identifier vs variable, prior miss), `#variable-mutation` (carrying forward stale values, prior miss). Plus fresh material from §Common Errors and §Identifiers.

**Grader contract:** never reveal answers • probe assumptions • reflect-contradiction • track hint level 0–3.

#### The 5 Questions (committed up front — no swapping)

**Q1 — Vocabulary precision.** What is the textbook's parent category — the exact term — for the names `userAge` (a variable), `main` (a function), and `Student` (a class)? One word.

**Q2 — Trace prediction.** Given the code below, what are the final values of `a`, `b`, `c`?

```cpp
int a = 3;
int b = 5;
int c = 2;

c = a + b;
b = a * c;
a = b - c;
```

**Q3 — Error spotting (no intent given).** For each of these three assignment lines *in isolation*, classify it as `compile-error`, `compiles-fine`, or `cannot-tell-without-more-info`. Justify the third category if you use it.

```cpp
1)  5 = totalScore;
2)  numFlights + 3 = totalSeats;
3)  numCats = numDogs;
```

**Q4 — Identifier legality.** For each candidate, say `legal` or `illegal`. If illegal, name the rule it breaks. If legal, note anything sneaky.

```
a) _age      e) numChairs2
b) 2x        f) class
c) int       g) total cost
d) total_cost h) Total   (assume `total` exists in the same scope)
```

**Q5 — Code construction.** Write the body of `main()`. No `cout` lines.

> Read an integer `numApples` from input. The user adds **2 more apples** to the basket. Then a friend gives them **the same number of oranges as apples currently in the basket**. Finally, store the **total fruit count** in `totalFruit`.

#### Attempt log

**Q1 — Vocabulary precision (identifier vs variable)**
- Attempt 1: "indentifiers" → accept (correct concept; flagged spelling typo: *identifier*)
- Resolved at hint level: **0**
- Grader self-assessment: right (concept-level — no nudge needed)
- Note: I&L `#vocabulary` retest — the term **stuck**.

**Q2 — Trace prediction (variable-mutation trap)**
- Attempt 1: `a=3, b=6, c=8` → probe ("walk me through `b=6` — what two values did you multiply?")
- Attempt 2: "a was 3 and c is 2 so 3*2 = 6" → reflect contradiction ("your final answer says c=8 but you used c=2 to compute b — both can't be true at once")
- Self-correction: "oh i see what I did wrong, I'm still computing the initial value"
- Attempt 3: `a=16, b=24, c=8` → accept
- Resolved at hint level: **2** (sustained Socratic, no values revealed)
- Grader self-assessment: right — the contradiction-reflection move is the win. Never had to say "c is 8 here." Learner found it in their own words.

**Q3 — Error spotting (lvalue rule + intent/silent-bug)**
- Attempt 1: "1. Compile error. 2. Compile error. 3. Compiles fine" → 1 & 2 accepted (concept-level), pushed on 3 ("why might `cannot-tell` be an option?") and asked for the rule behind 1 & 2
- Attempt 2: "I'm assuming the values have been initialized... the variable is supposed to be on the left" → concept right but wrong axis on (3); asked for the precise term for "what can go on the left"
- Attempt 3: "1 and 2: identifier. 3: we don't know if numDogs has a value at all" → "identifier" too broad (counter: `const int MAX`); pushed harder on 3 with yes/no thought experiment
- Attempt 4: "1 and 2: integer. 3: type mismatch?" → both wrong axis; sharper probe on each
- Attempt 5: "1 and 2: variable. 3: no" → **variable** accepted (textbook table answer); noted `lvalue` as the precision term. **No** on 3 accepted; pushed: "what's the only thing that differs between programmer A (correct) and programmer B (typo)?"
- Attempt 6: "is it an infinite loop of assignment. The values will never resolve correctly because they're setting each other" → **walk-away**. Surfaced intended angle (*intent*) without revealing earlier; flagged regression on `=` semantics ("`=` does not create a permanent link" — the learner's own notes contradict their attempt-6 framing)
- Resolved at hint level: **3 (walked away)**
- Grader self-assessment:
  - Right on the lvalue probe — got there via "variable" (textbook table answer); precision term `lvalue` deferred
  - **Useful failure on (3):** surfaced two real gaps — (a) the *intent vs. syntax* distinction didn't land, (b) regression on `=` semantics under pressure
  - The walk-away protocol felt clean — gave the *category* of what was being tested + pointed back to the learner's own notes, no literal answer revealed

**Q4 — Identifier legality (8 candidates)**
- Attempt 1: `a:legal, b:legal, c:illegal, d:legal, e:legal, f:illegal, g:illegal, h:illegal` → 6/8 correct; probed two: (b) 2x ("constraints on the first character?") and (h) Total ("re-read the case-sensitivity row")
- Attempt 2: "B: you can't start a variable name with a number. H: legal" → both self-corrected ✓
- Follow-up rule-naming probe on c/f/g → "term used in code, part of C++ language" (concept right) → asked for precise textbook term
- Final: "reserved words" → ✓ (also accepted: keywords)
- Resolved at hint level: **1**
- Grader self-assessment: right — short, precise. The "re-read the row in your notes" framing worked.

**Q5 — Code construction (variant of furniture exercise: apples + oranges + total)**
- Attempt 1:
  ```cpp
  int numApples;
  cin >> numApples;
  numApples + 2;                            // BUG: dangling expression
  numOranges = numApples;                   // BUG: not declared
  totalFruit = numApples + numOranges;      // BUG: not declared
  ```
  Two failure modes: (a) increment pattern from §Variables and Assignments not applied — `numApples + 2;` discards the result; (b) declare-before-use violated.
- Probe 1 ("walk me through `numApples + 2;` — what's `numApples` after that line?")
- Probe 2 ("where are `numOranges` and `totalFruit` first declared?")
- Attempt 2 (full rewrite, both bugs fixed):
  ```cpp
  int numApples;
  int numOranges;
  int totalFruit;
  cin >> numApples;
  numApples = numApples + 2;
  numOranges = numApples;
  totalFruit = numApples + numOranges;
  ```
  Order is correct (increment first, then copy).
- Resolved at hint level: **1** (one round, two probes — both bugs surfaced and fixed in single rewrite)
- Grader self-assessment: right — the two-probe strategy worked because the bugs were *independent*. Bundling them in one message kept the loop fast.

#### Retrospective

**Learner's verdict:** "Super helpful — and you didn't give me the answer even when I fully didn't answer correctly."

**Hint-level distribution:** 0, 2, 3 (walk-away), 1, 1 → mean ~1.4. The walk-away was the *most informative* result: it surfaced two real gaps (the term `lvalue` not retrievable from the learner's own notes; regression on `=`-as-action under pressure) that a quiz-with-answers would have papered over.

**Decision:** Re-entry coach next session. 3-question warmup weighted to today's two surfaced gaps.

#### Per-question grader self-assessment

| Q | Hint | Grader move that mattered |
|---|---|---|
| 1 | 0 | Accept the concept, flag spelling — don't punish typos |
| 2 | 2 | Reflect the learner's contradiction back in their own words ("you said c=8 AND used c=2 — pick one") |
| 3 | 3 (walk-away) | Walk-away protocol: surface the *category* of the missed concept (intent), point to the learner's own notes, never reveal literal answer |
| 4 | 1 | "Re-read the row in your notes" framing — short, precise |
| 5 | 1 | Bundle independent probes in one message when bugs are independent — keeps loop fast |
