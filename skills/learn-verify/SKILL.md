---
name: learn-verify
description: Step 3 of the Specificity Method. Verify AI answers with six accuracy checks — citations, citation verification, triangulation, date-check, uncertainty probing, and what's-missing — before trusting them. Use after decomposing a topic, when an answer sounds confident but unverified, when fact-checking or hunting hallucinations, when the user asks "are you sure?", "double-check your sources", "verify that", or "is this still true in 2026?", or as step 3 of learn-anything. The model pattern-matches; treat every claim as a hypothesis until checked.
---

# Step 3 — Verify Everything

AI is a professional-sounding pattern matcher, not a reasoner. An answer that "looks like" a good answer is not a true one. Trust it less; verify. Use as many checks below as the stakes warrant — more checks, more accuracy. **Skip these and the whole method collapses.**

Run these against every unknown ✗ piece from Step 2. If `deep-research` is installed, use it as the research engine and apply these six as the acceptance bar.

## The 6 accuracy checks

1. **Demand citations.** No source = hypothesis, not fact. Make the model attach where each claim came from.
2. **Verify the citation.** Open the page and read it. Don't trust the model's summary of its own source — summaries drift from what the page actually says.
3. **Triangulate.** One blog ≠ truth. Cross official docs + a GitHub repo + community (Reddit/forums) + optionally a different AI model. Where independent sources agree → higher confidence. A second model may surface biases or gaps Claude missed.
4. **Date-check 2026.** Training data is months stale; Claude Code ships in weeks. Tell the model "check as of today in 2026." Many problems are already solved — or changed — since the training cutoff.
5. **Probe uncertainty.** Ask "what would falsify this?" If a claim can't be falsified, it's pattern-matching, not knowledge. Also ask point-blank: "Are you 100% sure? Do one final sweep." It nearly always finds something.
6. **Ask what's missing.** "What would I need to know that you haven't told me?" A second pass routinely turns up sources, caveats, or edge cases the first missed.

## Output

For each piece: the verified claim, the sources that triangulate it, a confidence level, and the date checked. Write to `learn-state.md` under `## Verified facts`.

Next: hand off to `learn-reconstruct`.
