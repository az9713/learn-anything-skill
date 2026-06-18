# learn-anything-skill

A set of [Claude Code](https://docs.claude.com/en/docs/claude-code) skills that
turn vague confusion into verified, tested understanding — an implementation of
the **"Specificity Method"** for learning anything 10x faster.

Based on the video:
**[How I Use Claude Code to Learn Anything 10x Faster](https://www.youtube.com/watch?v=CJ91YJ6GvN4)**.
All credit for the method goes to the video's host; this repo only encodes the
five-step loop as reusable skills.

## The idea

Specificity is the whole game. You can't solve a fuzzy feeling ("I don't get AI
agents"), but you can solve a precise one ("I don't know why this agent runs a
planning step before every tool call"). The method moves you from
*"I don't know what I don't know"* to a mental model you've verified and proven
with a real test.

```
1. ARTICULATE  → turn the fog into one specific "I don't know X"
2. DECOMPOSE   → break X into pieces; mark each known ✓ / unknown ✗
3. VERIFY      → run 6 accuracy checks on every ✗
4. RECONSTRUCT → explain it back in your own words; rule fake vs real
5. APPLY+TEST  → Predict → Run → Compare → Fork

   match    → understood. exit the loop. ship it.
   mismatch → a precise gift. re-enter at step 2 and loop.
```

Failure isn't a fallback — it's part of the system. A mismatch tells you exactly
which piece you misunderstood, so you loop back to it.

## The skills

| Skill | Step | Role |
|-------|------|------|
| `learn-anything` | — | **Orchestrator.** Entry point. Runs the 5-step loop and carries state in `learn-state.md`. |
| `learn-articulate` | 1 | Vague feeling → one specific "I don't know X". |
| `learn-decompose` | 2 | Break the topic into pieces; the unknown ✗ pieces become the real questions. |
| `learn-verify` | 3 | Six accuracy checks: citations, verify the citation, triangulate, date-check, probe uncertainty, ask what's missing. |
| `learn-reconstruct` | 4 | Explain it back in your own words; judge real understanding vs faking. |
| `learn-apply-test` | 5 | Predict → Run → Compare → Fork. Prove the model with a real test. |

Each skill triggers on its own — ask to "verify these claims" and `learn-verify`
fires standalone — but `learn-anything` owns the full journey and dispatches the
rest as a loop.

### The six accuracy checks (step 3)

The model is a confident pattern matcher, not a reasoner. Skip these and the
whole method collapses.

1. **Demand citations** — no source = hypothesis, not fact.
2. **Verify the citation** — open the page; don't trust the summary.
3. **Triangulate** — official docs + GitHub + community; agreement = confidence.
4. **Date-check** — training data is stale; tools ship in weeks. "Check as of today."
5. **Probe uncertainty** — "what would falsify this?" If nothing can, it's pattern-matching.
6. **Ask what's missing** — a second sweep routinely finds caveats the first missed.

## Install

Copy the skill directories into your global or project skills folder:

```bash
# Global (all sessions)
cp -r skills/learn-* ~/.claude/skills/

# Or per-project (this repo's project only)
cp -r skills/learn-* /path/to/project/.claude/skills/
```

Skills load at session start, so restart Claude Code (or open a new session)
after copying. Then just describe what you're confused about — the orchestrator
triggers on phrases like *"I don't know what I don't know about X"*,
*"teach me X properly"*, or plain frustration with a concept.

## Usage example

```
I don't know what I don't know about how Claude Code subagents save context.
I build agents for clients and I need to actually understand this.
```

`learn-anything` fires, creates `learn-state.md`, then walks you through
articulate → decompose → verify → reconstruct → apply+test, looping back on any
mismatch until a test proves the mental model.

## Testing

See [TESTING.md](TESTING.md) for trigger tests (which prompt fires which skill),
a full-loop walkthrough with pass criteria, and an automated `claude -p`
trigger check.

## How it was built

Drafted as six `SKILL.md` files, reviewed against Claude Code's `skill-creator`
guidelines, and trigger-optimized by hand — descriptions tuned so the
orchestrator owns cold-start, each leaf owns its step, and none shadows the
others or the unrelated built-in `learn` skill. Triggering was confirmed
empirically with a `claude -p` spot-check: the cold-start query invokes
`learn-anything`.

## Credit & license

Method: from **[How I Use Claude Code to Learn Anything 10x Faster](https://www.youtube.com/watch?v=CJ91YJ6GvN4)**.
This repo is an independent skill implementation of that method and is not
affiliated with or endorsed by the video's host.

MIT License.
