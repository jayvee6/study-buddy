# study-buddy — Socratic Study Coach for Claude Code

A Claude Code skill that turns your Obsidian vault into a Socratic tutor with persistent per-learner memory. Generates source-grounded quiz questions calibrated to your documented weak spots, grades Socratically (never reveals the answer — even on walk-away), and persists every attempt back into your notes so the next session knows what you got wrong last time.

> **Built for discontinuous learners** — part-time students, working professionals, returning learners. The kind of person who comes back to a course after 3, 7, 14 days and needs to re-load context fast.

---

## Why

A generic LLM can generate quizzes. It cannot remember what *you specifically* got wrong last week.

The persistent log is the moat. Every design choice in this skill protects it: source-grounded questions, weak-spot calibration, the no-reveal grader contract, the dive-deeper-vs-continue branch, and the dated `## Quiz Attempts` capture format that lets the next session pick up where this one left off.

---

## What it does

Four modes, picked from the trigger phrase:

| Mode | Trigger | Shape |
|---|---|---|
| **Standard session** | "quiz me on X", "test me on chapter 4" | N questions (default 5), full Socratic loop, full capture |
| **Re-entry coach** | "warm me up", "re-entry", or any session after 3+ days gap | 3 questions weighted to last session's failure modes, ≤5 min |
| **Mid-class drop-in** | "quick check", "drop-in", or invoked during active note-taking | 1–2 questions on the *just-captured* concept, minimal ceremony |
| **Diagnostic** | "what did I get wrong last time on X?" | Reads past attempts + weak-spots ledger, summarizes patterns |

The grader holds a **hard no-reveal rule**. Not on attempt 3. Not on walk-away. Even when the learner taps out, the skill surfaces the *category* of the missed concept and points back to the learner's own notes — never the literal answer. This is the riskiest assumption of the whole skill, and it's what differentiates the loop from "a textbook quiz with extra steps."

---

## Install

```bash
git clone https://github.com/jayvee6/study-buddy.git ~/.claude/skills/study-buddy
```

Restart Claude Code. The skill auto-registers from `~/.claude/skills/study-buddy/SKILL.md`. Trigger it with any of the phrases in the table above.

### Per-project install

If you want it scoped to one project instead of user-global:

```bash
git clone https://github.com/jayvee6/study-buddy.git <project>/.claude/skills/study-buddy
```

---

## Usage

Just talk to Claude. The skill triggers on natural phrasing — you don't have to invoke it explicitly:

```
quiz me on chapter 4 of the bio textbook
```

```
warm me up before class — last session was on probability distributions
```

```
quick check on this concept I just wrote down
```

```
what did I get wrong last time on functional groups?
```

The skill will:

1. **Read what's available** — your source note, any weak-spots ledger, past `## Quiz Attempts`.
2. **Prompt you for what's missing** (terse, only what it genuinely needs — e.g., "How many questions?" or "Should I lean on a specific weak spot?").
3. **Commit the question set up front** — writes all N questions into your source note's `## Quiz Attempts` section *before* presenting the first one. No after-the-fact swapping.
4. **Run the Socratic loop** — one question at a time. After each closes, offers `dive deeper` or `continue`.
5. **Capture everything** — attempt log, hint level (0–3), grader self-assessment, your verdict.

---

## Vault conventions

Built for an Obsidian vault layout but generalizes to any markdown-notes setup:

- **Source notes:** wherever your canonical content lives. The skill appends to a `## Quiz Attempts` section there.
- **Weak-spots ledger** *(optional)*: a separate note (e.g., `Issues & Learnings.md`) listing past mistakes with tags. If you don't have one, the skill reads past `## Quiz Attempts` from the same source note.

If your vault doesn't follow this layout, the skill will ask once where things live, then proceed.

---

## Example output

A fully-worked session — 5 questions, hint distribution 0/2/3/1/1, learner verdict and per-question grader self-assessment — lives at [`references/example-session.md`](references/example-session.md). Read it for format and concrete grader-move examples.

The walk-away on Q3 is the most instructive part: the grader held the no-reveal line for 6 attempts, surfaced two real conceptual gaps (`lvalue` term not retrievable + regression on `=`-as-action), and pointed the learner back to their own notes — without ever revealing the literal answer. That's the contract working.

---

## Status

**v0** — markdown-only loop, no scripts, no HTML. Earned by a real dogfood test (one learner, one CS module, 7-day gap). The riskiest assumption (the Socratic grader can hold no-reveal at walk-away and still feel useful, not punishing) was tested and passed.

**Parked for later versions:**
- HTML quiz interface (v1+)
- Interactive code playgrounds (v2+)
- Three.js viz for geometry/physics (v3+)
- Multi-user / classroom mode

If the loop works for you, open an issue with what's missing. If it doesn't, open one with why.

---

## License

No license set yet — treat as all-rights-reserved for now. Issues + PRs welcome.
