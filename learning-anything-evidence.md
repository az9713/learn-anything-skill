# Evidence: the skills trigger and the loop completes

Two pieces of evidence that this skill set works as designed — one for
*triggering* (the right skill fires) and one for the *full loop* (a learning
session runs end to end and exits on a passing test).

## 1. Triggering — `learn-anything` fires on a cold-start prompt

A nested headless session was run with the cold-start query:

> "I don't know what I don't know about Rust async runtimes. help me actually understand it"

Command:

```bash
claude -p "I don't know what I don't know about Rust async runtimes. help me actually understand it" \
  --output-format stream-json --verbose --model claude-opus-4-8
```

The streamed tool-use event captured was:

```json
{"name":"Skill","input":{"skill":"learn-anything","args":"Rust async runtimes"}}
```

**Result:** the orchestrator `learn-anything` fired — not the unrelated built-in
`learn` skill, and not any individual step skill. The five step skills appeared
only in the available-skills listing, never invoked. Boundary design holds:
cold-start goes to the orchestrator.

## 2. Full loop — a session reaching "understood"

[`learn-state.md`](learn-state.md) is the scratch file produced by a complete
run of the loop (topic: *writing an effective SKILL.md*). It shows every step
filled in and the loop exiting on a passing test:

- **Gap** — narrowed to a single specific unknown: which parts of a SKILL.md
  have rules vs. principles.
- **Decompose** — six pieces, each tracked.
- **Verify** — claims pinned to sources (`skill-creator` SKILL.md +
  `quick_validate.py`), date-checked 2026, confidence noted.
- **Reconstruct** — the learner's own-words model judged REAL (not faked).
- **Apply + test** — a concrete prediction, a real test run
  (`quick_validate.py` on two sample skills), result compared, **fork = MATCH**.
- **Current step: DONE (understood + tested).**

This is exactly the intended shape: the loop doesn't stop at a confident-sounding
explanation — it exits only when a real test confirms the mental model.

## Known limits

These runs were on Windows. The skill-creator *automated* optimization loop
(`run_loop.py` / `run_eval.py`) was not used — its improve step needs an
Anthropic API key and its eval harness uses POSIX-only pipe `select()`. The
triggering evidence above is the manual `claude -p` substitute. See
[TESTING.md](TESTING.md) for the full notes.
