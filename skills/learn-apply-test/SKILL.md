---
name: learn-apply-test
description: Step 5 of the Specificity Method. Prove a mental model with a real test — Predict, Run, Compare, Fork. Use after the user can explain a concept in their own words, when learning needs proof in practice, or as the final step of learn-anything. A mismatch is a precise gift that points at the misunderstood piece; loop back on it.
---

# Step 5 — Apply and Test

Learning is only complete when a test changes (or confirms) the mental model. Reconstructing it in your head isn't proof — running it is.

## The loop

```
1. PREDICT  → what will happen, based on the model reconstructed in Step 4.
2. RUN      → the minimum viable real test. Smallest thing that exercises the claim.
3. COMPARE  → did the result match the prediction?
4. FORK     → match    → understood. exit the loop. ship it.
              mismatch → stay in the loop. re-decompose, re-predict.
```

## How

1. **Predict out loud first.** Write the expected outcome to `learn-state.md` *before* running. Predicting after the fact proves nothing.
2. **Build the smallest real test.** Ask Claude to spin up a minimal environment / orchestrated run that exercises exactly the claim — not a full system. For the subagent example: two implementations of one task, one in-main vs one delegated, compare tokens/quality/context loss.
3. **Compare honestly** against the written prediction.
4. **Fork:**
   - **Match** → the model holds. Summarize it, mark the topic understood, exit.
   - **Mismatch** → a *precise gift*. It names the broken piece. The research or the model was wrong there. Return to Step 2 (`learn-decompose`), fix that piece, and run the loop again.
5. **Output:** prediction, what ran, result, fork decision. Write to `learn-state.md` under `## Test`.

A mismatch isn't failure — it's the loop doing its job. Loop until match.
