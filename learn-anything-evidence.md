# Evidence: How `/learn-anything` Works — A Turn-by-Turn Case Study

**Topic learned:** What makes an effective Claude `SKILL.md` (and why it has "no grammar")
**Date:** 2026-06-17
**Method:** The Specificity Method (5-step loop), run by the `learn-anything` orchestrator skill
**Outcome:** Unknown-unknown ("I don't know what I don't know") → one specific gap → verified against primary sources → user reconstructed it → **proven by a live test whose result matched the user's prediction.** Loop exited on a MATCH.

---

## The method at a glance

```
1. ARTICULATE  → turn the fog into one specific "I don't know X"
2. DECOMPOSE   → break X into pieces; mark each known ✓ / unknown ✗
3. VERIFY      → ground every ✗ in authoritative sources (not memory)
4. RECONSTRUCT → user explains it back; rule fake vs real
5. APPLY+TEST  → Predict → Run → Compare → Fork
       match    → understood, exit loop
       mismatch → re-decompose the broken piece, loop again
```

A persistent file, `learn-state.md`, is the loop's memory and is rewritten after each step.

---

## Turn 0 — Trigger

**User said:**
> "I dont know what I dont know about Claude skills"

**What happened:** This phrasing ("I don't know what I don't know about X") is a direct trigger for `learn-anything`. The skill was invoked before any other response. The orchestrator's first action was to create the loop's memory file `learn-state.md` (it found a completed prior run about "Workflow" and repurposed the file for the new topic).

**Loop step entered:** 1 (ARTICULATE).

---

## Turn 1 — ARTICULATE (round 1 of 3): give a map, localize the fog

**Why this move:** "I don't know what I don't know" is an *unknown unknown*. You cannot decompose a feeling. The fastest way to convert it into a *known unknown* is to hand the learner a compact map of the territory and ask them to point at the foggiest region.

**Assistant did:**
- Gave a 3-bullet orientation: (a) a skill = a folder with a `SKILL.md`; (b) the defining trick is *progressive disclosure*; (c) skills are one of five oft-confused mechanisms (skills / slash commands / subagents / MCP servers / hooks).
- Asked a structured question (`AskUserQuestion`) with 4 regions to localize the fog:
  1. What a skill fundamentally IS
  2. How Claude decides to USE one
  3. **How to CREATE my own**
  4. Managing the ones I have

**User chose:** "How to CREATE my own."

**State:** Gap still `<pending>`; fog narrowed from "everything" to "authoring."

---

## Turn 2 — ARTICULATE (round 2 of 3): drill into the chosen region

**Why this move:** "Creating a skill" is still several gaps in a trench coat. The method demands *one specific sentence*, so we drill again.

**Assistant did:** Asked a second `AskUserQuestion` with 4 sub-regions of authoring:
1. The mechanics / minimum viable skill
2. Writing a description that triggers
3. What goes in the body
4. Knowing it actually works

**User did something better than pick an option — they articulated the gap themselves:**
> "unlike programming language, there is no formal grammar for SKILL.md. I do not know what sections are needed and what to put into each section. also, those sections or contents vary from SKILL to SKILL. i feel like i have no rules or constraints to hold onto to write a good and effective skill file."

**This is the whole point of Step 1.** The vague fog became a sharp, solvable sentence. The learner's own words located the *exact* misconception (expecting one grammar for the whole file).

**State (`learn-state.md`) updated — Gap recorded:**
> SKILL.md has no formal grammar like a programming language. Which sections are required? What goes in each? Why do they vary skill-to-skill? I feel I have no rules/constraints to hold onto.

**Loop step:** 1 → complete.

---

## Turn 3 — DECOMPOSE: break the gap into checkable pieces

**Why this move:** A precise gap can be split into pieces, each marked known/unknown. This reveals *which* sub-fact to verify and exposes the load-bearing misconception (the "crux").

**Assistant did:** Wrote 6 pieces into `learn-state.md`, all marked unknown ✗:

- **A. CRUX** — a SKILL.md is TWO layers: frontmatter (strict) + body (freeform). The confusion = expecting one grammar for both.
- **B.** The frontmatter *does* have a grammar (required `name` + `description`, format constraints).
- **C.** The body has *no* syntax grammar, but *does* have design **principles** — the "rules to hold onto" are principles, not syntax.
- **D.** The load-bearing principle = **progressive disclosure** (keep SKILL.md short, push detail to referenced files).
- **E.** Why sections vary: the body = task instructions; tasks differ, so only *patterns* recur (overview / when-to-use / workflow / examples / references).
- **F.** A concrete checklist of what makes a skill *effective*.

**Loop step:** 2 → complete. Crux identified = A + C (looking for a grammar in the wrong layer).

---

## Turn 4 — VERIFY: ground every claim in primary sources, not memory

**Why this move:** The learner's entire anxiety was "what are the *actual* rules?" Answering from memory would betray that. The method's verify step demands authoritative sources.

**Assistant did:**
1. `Glob` to locate the **official `skill-creator` skill** installed locally (Anthropic's own source of truth).
2. `Read` the canonical `skill-creator/SKILL.md` (486 lines). Confirmed: anatomy (frontmatter required + optional `scripts/`/`references/`/`assets/`), `name`+`description` required, description = primary triggering mechanism, progressive disclosure 3-level model, body has *patterns not required sections* ("the rest of the skill :)").
3. `Read` the validator script `quick_validate.py` — **a validator is a formal grammar written as code.** This was the decisive source.

**Smoking-gun finding from the validator:** it checks the **frontmatter** rigorously and checks the **body zero times.** That asymmetry *is* the answer to the gap.

**Verified frontmatter grammar (from `quick_validate.py`), HIGH confidence:**
- File must be `SKILL.md`, must start with `---`, frontmatter must parse as a YAML dict.
- Allowed keys (whitelist — anything else fails): `name`, `description`, `license`, `allowed-tools`, `metadata`, `compatibility`.
- Required: `name`, `description`.
- `name`: kebab-case `^[a-z0-9-]+$`, no leading/trailing/double hyphen, ≤ 64 chars.
- `description`: no angle brackets, ≤ 1024 chars.
- `compatibility` (optional): string ≤ 500 chars.

All 6 decomposed pieces were confirmed and written into `learn-state.md` with sources + confidence.

**Loop step:** 3 → complete.

---

## Turn 4 (cont.) — TEACH the verified model

**Assistant delivered the resolution** in the learner's frame ("your instinct was correct — there IS no grammar for *most* of the file; the mistake was assuming there should be one for the *whole* file"):

- **The seam:** a SKILL.md is two glued-together things — frontmatter (Layer 1) and body (Layer 2) — split at the closing `---`.
- **Layer 1 table** of the 6 keys / 2 required / exact constraints.
- **Layer 2 principles** (not syntax): `description` carries all the "when"; progressive disclosure keeps the body lean; write imperatively + explain *why*; recurring patterns, not required sections.
- **The reframe:** frontmatter = a tiny strict form; body = a short, organized memo to a smart colleague.

---

## Turn 5 — RECONSTRUCT: learner explains it back; rule fake vs real

**Why this move:** Hearing an explanation ≠ owning it. The learner must reproduce it in their own words so the teacher can tell real understanding from a nod-along.

**Assistant asked:** "If a friend said 'SKILL.md has no rules, it's a free-for-all,' how would you correct them in 3-4 sentences — which part has hard rules, which doesn't, and what do you hold onto for the part that doesn't?"

**User answered (terse but structurally correct):**
> "the formatter has rules. the body does not. principles for the body."

**Verdict: REAL on the crux** — the learner nailed the central seam, which *was* the actual gap. But "principles for the body" was still a placeholder. Rather than accept a placeholder, the assistant moved straight to the live test, which forces the specifics to surface.

**Loop step:** 4 → complete (real, pending test confirmation of specifics).

---

## Turn 6 — APPLY + TEST: Predict → Run → Compare → Fork

**Why this move:** The only non-negotiable rule of the method: never declare "understood" without a test passing. A test that matches a prediction is proof; a mismatch is a precise gift showing which piece broke.

**Assistant built two adversarial test skills** (`Write`), each designed to attack the model from one side:

| Skill | Body | Frontmatter | Designed to test |
|-------|------|-------------|------------------|
| A `garbage-body` | literal nonsense (`asdkjfh %%%%%`, no headings) | clean, valid | "body has no rules" |
| B `nice-body` | textbook structure (overview/when/workflow/examples) | `name: Nice_Body` (illegal) | "frontmatter has rules" |

**PREDICT — assistant asked the user to commit first. User predicted:**
> "A passes, B fails — name must be kebab-case"

**RUN — assistant executed the official validator** (`Bash`) on both:

```
=== Skill A: garbage-body ===
Skill is valid!
(exit 0)
=== Skill B: nice-body ===
Name 'Nice_Body' should be kebab-case (lowercase letters, digits, and hyphens only)
(exit 1)
```

**COMPARE:** prediction matched reality exactly.
- A (garbage body, clean frontmatter) → **valid**, exit 0.
- B (perfect body, bad `name`) → **FAIL**, exit 1, on the kebab-case rule.

**FORK: MATCH → understood. Loop exited.**

The result is decisive evidence precisely because it is counter-intuitive: a skill whose entire body is gibberish *passes*, and a beautifully written skill *fails* on one uppercase letter. The learner's mental model predicted the machine's exit codes. That is the operational definition of understanding the method uses.

**Loop step:** 5 → complete. Stop condition met (understood + test proves it).

---

## What `learn-anything` actually did differently (vs. "just answer the question")

1. **Refused to answer the vague question.** It would have been easy to dump "here's how to write a SKILL.md." Instead it spent two turns turning fog into one precise, learner-owned sentence. *Specificity is the whole game.*
2. **Found the misconception, not just the topic.** The decompose step isolated the *crux* (looking for one grammar across two layers) rather than lecturing all of "skill authoring."
3. **Verified against primary sources.** It read Anthropic's own `skill-creator` skill and — crucially — the **validator code**, because a validator literally *is* the formal grammar the learner wanted. No reliance on the model's memory.
4. **Made the learner reconstruct.** Comprehension was checked, not assumed.
5. **Proved it with a falsifiable test.** A prediction + a real command + a matching result. Not "seems right" — exit codes.

## The loop's memory (`learn-state.md`) — final state

The companion file `learn-state.md` shows the same journey as machine-readable state: Gap → 6 Pieces → Verified facts (with sources) → Mental model (verdict: REAL) → Test (Prediction / Ran / Result / Fork: MATCH) → `Current step: DONE`.

## Artifacts produced this session
- `learn-state.md` — the loop's living memory, rewritten each step.
- `skill-test/garbage-body/SKILL.md` — valid frontmatter + nonsense body (passes validation).
- `skill-test/nice-body/SKILL.md` — perfect body + illegal `name` (fails validation).
- `learn-anything-evidence.md` — this document.
