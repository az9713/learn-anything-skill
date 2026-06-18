---
name: learn-articulate
description: Step 1 of the Specificity Method. Turn a vague feeling ("I don't get AI agents", "this confuses me", "I can't even phrase my question") into one specific, actionable "I don't know X". Use when the user's question is too fuzzy to act on, when they ask to "get specific", "narrow this down", or pin down what they're actually missing, or as the first step of learn-anything. For a full learning journey from confusion to tested understanding, prefer the learn-anything orchestrator instead. Researches the fog if the gap can't yet be named.
---

# Step 1 — Articulate the Gap

The step almost everyone skips. A vague statement gives vague answers; a specific one gives a clear, actionable problem.

> Vague / useless: "I don't get AI agents." "This confuses me." "The docs are bad."
> Specific / useful: "I don't know why this agent runs a planning step before every tool call." "I don't know when a subagent saves context vs. adds overhead."

## How

1. **Interrogate the fog.** Ask the user pointed follow-ups until the confusion collapses to one sentence: *what are they building, with what, and what decision is blocked by not knowing?* If `superpowers:brainstorm` or `idea-refine` is installed, use it — its questioning is built for exactly this.
2. **Don't stop at the first sentence.** The first specific question usually sits on top of deeper ones. That's fine — those surface in Step 2.
3. **Stuck and can't even name the gap?** Research the fog first: have the model give a rough map of the territory, then point at the part that feels dark. Name *that*.
4. **Output:** one sentence of the form "I don't know X." Write it to `learn-state.md` under `## Gap`.

Specificity turns a feeling into a problem. A problem can be solved.

Next: hand off to `learn-decompose`.
