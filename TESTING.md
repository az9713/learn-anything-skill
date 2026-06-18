# Testing the Specificity Method skills

Six skills implement the 5-step learning loop from the video
"How I Use Claude Code to Learn Anything 10x Faster":
`learn-anything` (orchestrator) + `learn-articulate`, `learn-decompose`,
`learn-verify`, `learn-reconstruct`, `learn-apply-test` (one per step).

Installed at:
- Global: `~/.claude/skills/learn-*` (all sessions)
- Project: `.claude/skills/learn-*` (this folder)

Skills are invoked by description match, not by command. To test, open Claude
Code, paste a prompt, and watch for a `Skill(learn-...)` line — that line means
the skill triggered.

## A. Trigger tests — does the right skill fire?

| Prompt | Should fire |
|--------|-------------|
| `I don't know what I don't know about Kafka consumer groups. help me actually get this` | `learn-anything` |
| `teach me how Postgres MVCC works, properly` | `learn-anything` |
| `my question about JWT refresh is too vague — help me get specific` | `learn-articulate` |
| `break "Kubernetes networking" into the pieces I'd need to understand` | `learn-decompose` |
| `you just explained TCP backpressure — are you sure? double-check your sources, is this still true in 2026` | `learn-verify` |
| `let me explain RAII back to you and tell me if I actually get it` | `learn-reconstruct` |
| `let's test if I understand goroutine scheduling — predict then run something` | `learn-apply-test` |

### Negatives — should NOT fire these skills

- `show me our project learnings` → existing `learn` skill (manage project learnings), not these.
- `what's the syntax for a Rust match arm?` → too simple; direct answer, no skill.

## B. Full-loop example — the real test

Open a fresh session, paste:

```
I don't know what I don't know about how Claude Code subagents save context.
I build agents for clients and I need to actually understand this, not just get an answer.
```

Expected run:

1. `learn-anything` fires → creates `learn-state.md` in the working directory.
2. **Articulate** → narrows to e.g. *"I don't know when a subagent's fork saves context vs adds overhead."*
3. **Decompose** → pieces: fork, context window, tool access, handoff, verification — each marked known ✓ / unknown ✗.
4. **Verify** → 6 accuracy checks on each ✗ (citations, verify the citation, triangulate docs + GitHub + forum, date-check 2026, probe uncertainty, ask what's missing).
5. **Reconstruct** → it asks *you* to explain it back; it rules fake vs real.
6. **Apply + test** → predict → build a tiny real test → compare → fork (match = done, mismatch = loop back to step 2).

**Pass criteria:**
- `learn-state.md` exists and fills in gap → pieces → verified facts → mental model → test.
- On a wrong prediction it loops back to **decompose**, not forward.
- It never declares "understood" without step 5 passing.

## How to run each

1. Start a new Claude Code session (the skill list reloads at startup).
2. Paste one prompt.
3. Watch for the `Skill(...)` invocation line — that is the trigger.
4. For the full example, inspect state afterward: `cat learn-state.md`.

## Fast automated trigger check (optional, no fresh session)

```bash
cd /path/to/your/project
unset CLAUDECODE
claude -p "teach me how Postgres MVCC works, properly" \
  --output-format stream-json --verbose --model claude-opus-4-8 2>/dev/null \
  | grep -oE '"skill":"[a-z-]+"'
```

Prints the skill that fired, e.g. `"skill":"learn-anything"`. Swap the prompt
for any row in section A. Each run spins up a full nested Claude session
(~2-4 min). Run from Git Bash; `unset CLAUDECODE` lets `claude -p` nest.

## Notes / known limits on this machine

- The skill-creator automated optimization loop (`run_loop.py`) is **not**
  runnable here: its improve step needs `ANTHROPIC_API_KEY` (none set on a Max
  subscription), and its eval harness (`run_eval.py`) uses POSIX pipe `select()`
  which fails on Windows. Run it on WSL/Linux with an API key if you want the
  full automated sweep. The single `claude -p` check above is the manual
  substitute.
