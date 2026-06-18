---
name: learn-anything
description: Learn any topic 10x faster using the Specificity Method — a 5-step loop that turns "I don't know what I don't know" into a verified, tested mental model. Use when the user is confused, stuck, says "I don't get X", "I don't know what I don't know about X", "help me understand X", "teach me X", "I want to learn/grok X", or expresses vague frustration or confusion about a concept/architecture/tool and wants to truly understand it rather than just get a quick answer — even if they never use the word "learn". This orchestrator owns the full journey: prefer it over the individual learn-articulate / learn-decompose / learn-verify / learn-reconstruct / learn-apply-test step skills whenever the user is starting fresh or wants end-to-end understanding. It runs those five as a loop.
---

# Learn Anything — The Specificity Method

Move the user from a fuzzy "I don't know what I don't know" to a specific, verified, tested mental model. Specificity is the whole game: a vague feeling cannot be solved; a precise "I don't know X" can.

This skill is the **orchestrator**. It runs five steps as a loop and carries state between them.

## The loop

```
1. ARTICULATE  → turn the fog into one specific "I don't know X"
2. DECOMPOSE   → break X into pieces; mark each known ✓ / unknown ✗
3. VERIFY      → run the 6 accuracy checks on every ✗
4. RECONSTRUCT → user explains it back in their own words; you rule fake vs real
5. APPLY+TEST  → Predict → Run → Compare → Fork

   match    → understood. exit the loop. ship it.
   mismatch → a precise gift. re-enter at step 2 (re-decompose) and loop.
```

Failure is not a fallback — it is part of the system. A mismatch tells you exactly which piece was misunderstood. Loop back.

## How to run it

1. **Create the state file** `learn-state.md` in the current working directory (template below). It is the loop's memory.
2. **Dispatch each step to its sub-skill** in order: invoke `learn-articulate`, then `learn-decompose`, then `learn-verify`, then `learn-reconstruct`, then `learn-apply-test`. After each, write its output into `learn-state.md`.
3. **At step 5, fork:**
   - Result matches the prediction → mark the topic understood, summarize the mental model, stop.
   - Result mismatches → record which piece broke, return to step 2 with that piece, run the loop again.
4. **Stop conditions:** topic understood and a test proves it, OR the user says stop. Never declare "understood" without step 5 passing.

## learn-state.md template

```markdown
# Learning: <topic>

## Gap (Step 1)
I don't know: <one specific sentence>

## Pieces (Step 2)
- [ ] piece A — unknown ✗
- [x] piece B — known ✓

## Verified facts (Step 3)
- <claim> — sources: <url>, <url> — confidence: <high/med/low> — checked: 2026

## My mental model (Step 4)
<user's own words> — verdict: real / faked-here: <piece>

## Test (Step 5)
- Prediction: <what should happen>
- Ran: <what was run>
- Result: <observed>
- Fork: match → done | mismatch → re-decompose <piece>

## Current step: <1-5>
```

## Notes

- Steps 1, 3, 4 lean on existing skills where installed (`superpowers:brainstorm` for articulate, `deep-research` for verify, `comprehension-gate`/`session-teacher` for reconstruct). The `learn-*` sub-skills wrap them with this method's flavor.
- Keep the user in the loop. The point is *their* mental model, not your prose.
