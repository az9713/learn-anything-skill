# Learning: Claude skills — writing an effective SKILL.md

## Gap (Step 1)
I don't know: SKILL.md has no formal grammar like a programming language. Which
sections are required? What goes in each? Why do they vary skill-to-skill? I feel
I have no rules/constraints to hold onto to write a good, effective skill.

## Pieces (Step 2)
- [ ] A. CRUX: a SKILL.md is TWO layers — (1) frontmatter = strict/required, (2) body = freeform prose. Confusion = expecting one grammar for both.
- [ ] B. The frontmatter DOES have a grammar: required `name` + `description`, with format constraints. This is the part with real rules.
- [ ] C. The body has NO syntax grammar (just Markdown Claude reads) but DOES have design PRINCIPLES — the "rules to hold onto" are principles, not syntax.
- [ ] D. Load-bearing principle = progressive disclosure: keep SKILL.md short, push detail to referenced files. Explains the whole design.
- [ ] E. Why sections vary: body = instructions for a task; tasks differ, so no fixed section list — only recurring PATTERNS (overview / when-to-use / workflow / examples / references).
- [ ] F. The actual checklist of what makes a skill EFFECTIVE (so you have constraints to hold).

## Verified facts (Step 3) — sources: official skill-creator SKILL.md + quick_validate.py, checked 2026, HIGH confidence
- A SKILL.md = (1) YAML frontmatter between `---` fences + (2) Markdown body. (SKILL.md anatomy, lines 75-84)
- Frontmatter is the ONLY part with a formal grammar. quick_validate.py enforces:
  - file named SKILL.md, must START with `---`, frontmatter must parse as a YAML dict
  - ALLOWED keys (whitelist): name, description, license, allowed-tools, metadata, compatibility
  - REQUIRED: name, description
  - name: kebab-case `^[a-z0-9-]+$`, no leading/trailing/double hyphen, <=64 chars
  - description: no angle brackets, <=1024 chars
  - compatibility (optional): string <=500 chars
- THE PROOF: the validator checks the body ZERO times. After the closing `---`, anything goes. Body has no grammar.
- description = primary triggering mechanism. Must state WHAT it does + WHEN to use. Be "pushy" (Claude undertriggers). (line 67)
- Progressive disclosure (3 levels): metadata ~100 words always loaded / SKILL.md body <500 lines when triggered / bundled resources as-needed. (lines 86-93)
- Body has PATTERNS not required sections: imperative voice, output-format templates, examples, "explain the WHY not heavy MUSTs". "the rest of the skill :)" = freeform. (lines 69, 115-139)
- Sections vary because body = task instructions; different tasks need different instructions. Recurring patterns only: overview / when-to-use(->desc) / workflow steps / examples / pointers to references.

## My mental model (Step 4)
- User: "frontmatter has rules. body does not. principles for the body." — verdict: REAL.
  Nailed the central seam (the actual gap). Specifics confirmed by the live test below.

## Test (Step 5)
- Prediction: A (garbage body, clean frontmatter) PASSES; B (great body, name=Nice_Body) FAILS because name must be kebab-case.
- Ran: quick_validate.py on skill-test/garbage-body and skill-test/nice-body.
- Result: A -> "Skill is valid!" exit 0. B -> "should be kebab-case" exit 1.
- Fork: MATCH -> understood. Loop exited.

## Current step: DONE (understood + tested)
