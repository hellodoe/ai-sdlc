---
name: to-intent
description: Condense the current conversation (usually after grill-me) into specs/SLUG/intent.md and open a PR for product-owner approval. Use when the user says to-intent or asks to save the intent.
disable-model-invocation: true
---

# to-intent

Turn what was just discussed into the Stage 1 artifact of our SDLC: `specs/<slug>/intent.md`. Merging the PR this skill opens is gate G1 (product owner approves intent).

Do NOT interview the user. Synthesize what is already in the conversation. If something below cannot be filled from the conversation, write `Open question:` with the question instead of guessing.

## Steps

1. Pick a short kebab-case slug for the work (for example `bulk-invoice-export`). If `specs/<slug>/` already exists, ask the user whether to update it or choose another slug.
2. If `specs/_template/intent.md` exists, follow its headings. Otherwise use the template below.
3. Write `specs/<slug>/intent.md`. Use the requester's own words for the problem wherever possible. Use the terms in `GLOSSARY.md` when it exists.
4. Keep it under one page. It states the problem and what done looks like; it does not design the solution. Design belongs in to-spec.
5. Create a branch `intent/<slug>`, commit the file, push, and open a PR titled `Intent: <one-line summary>`. Request review from the product owner if CODEOWNERS names one for `specs/`.
6. Tell the user the PR link and the next step: after approval, run grill-with-docs then to-spec, and link the resulting spec issue back into intent.md.

## Template

```markdown
# Intent: <one-line summary>

- Requester: <name or team>
- Source: <issue, ticket, alert or conversation link>
- Date: <YYYY-MM-DD>
- Risk tier: <low | medium | high> (high = auth, payments, data deletion, compliance)

## Problem
<Who has the problem, what happens today, in the requester's words.>

## Why now
<What it costs to leave it, and any deadline.>

## Done looks like
<Observable outcomes a reviewer could check. Bullets.>

## Out of scope
<What was explicitly ruled out during the grilling.>

## Decisions already made
<Branches resolved in the grilling, one line each.>

## Open questions
<Anything still unresolved. Empty if none.>

## Spec
<Link to the spec issue once to-spec has run.>
```
