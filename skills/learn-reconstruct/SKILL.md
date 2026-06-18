---
name: learn-reconstruct
description: Step 4 of the Specificity Method. Have the user explain the concept back in their own words, then judge fake vs real understanding. Use after research is verified, when checking whether someone actually understands something or is just reciting jargon, or as step 4 of learn-anything. Language is brutal — you either have the concept or you don't.
---

# Step 4 — Reconstruct in Your Own Words

The point was never to memorize AI phrasing. It's to build a mental model the user can explain and reuse. Explaining it back exposes superficial understanding instantly.

## How

1. **Ask the user to explain it back** — their own words, own terminology, own analogies. Not the docs' words, not Claude's words. "So what you're really saying is it works by X because Y depends on Z."
2. **Judge fake vs real:**
   - **Fake ❌** — vague paraphrase, reaches back to the source to fill a gap, hand-waves a piece. If they have to look it up mid-explanation, that piece is faked.
   - **Real ✓** — concrete, mechanism-level: "this works because A depends on B, which means C." No source needed; it's in their head.
3. **Critique honestly.** Tell them exactly where the explanation is right and where it's still wrong or incomplete. Language is brutal: a missing word usually means a missing concept.
4. **Faked a piece?** That piece goes back to Step 2 (`learn-decompose`) — re-decompose and re-verify it. Don't paper over it.
5. **Output:** the user's mental model in their words + verdict (real, or which piece faked). Write to `learn-state.md` under `## My mental model`.

A model in your own framework is far easier to remember and to teach than recited jargon.

Next: if real, hand off to `learn-apply-test`. If faked, return to `learn-decompose`.
