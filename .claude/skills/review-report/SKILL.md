---
name: review-report
description: Run the Stage 4 checks on the current branch (typecheck, full tests, Pocock code-review, security-review) and post one review report on the PR as the G4 artifact. Use when the user says review-report.
disable-model-invocation: true
---

# review-report

Produce the Stage 4 (Test) artifact of our SDLC: one review report per PR, posted as a PR comment so it lives next to the diff it judges. It is the evidence a human code owner reads before G5.

## Steps

1. **Find the inputs.**
   - Base: the PR's base branch, else `main`.
   - Spec: the issue referenced in the branch's commit messages or PR body. If none is found, ask the user for it; do not guess.
   - Ticket: the tracer-bullet ticket this branch implements, if referenced.
   - Commit: the current `HEAD` SHA.
2. **Run the checks** with the commands in CLAUDE.md. Capture counts, not full logs:
   - typecheck: pass or fail, with the number of errors
   - full test suite: passed, failed, skipped; coverage for changed files if the repo reports it
   - tests added or changed in this diff: list the test files and the seams they cover
3. **Run /code-review** against the base. Keep its two reports (Standards, Spec) as they come.
4. **Run /security-review** on the diff.
5. **Rank findings.** Each finding gets a severity: Important (blocks G4) or Nit (does not). Use REVIEW.md's definitions when it exists. Note which findings you fixed during this run and which remain open.
6. **Write the report** with the template below. Keep it under 600 words; link to CI logs rather than pasting them.
7. **Publish it.**
   - If a PR exists: post it with `gh pr comment`. If a previous comment contains the marker `review-report:v1`, edit that comment instead of adding a new one.
   - If no PR exists yet: print the report and tell the user it will be posted when the PR opens.
8. Tell the user the verdict in one line and what blocks G4, if anything.

Do not approve the PR. The report informs the human code owner; it never replaces their approval.

## Template

```markdown
<!-- review-report:v1 -->
## Review report: <ticket title>

Commit `<sha>` against `<base>` · Spec: <spec issue link> · Ticket: <ticket link>

**Verdict: <Ready for human review | Blocked>**

### G4 checklist
- [ ] Typecheck clean
- [ ] Full suite green (<passed> passed, <failed> failed, <skipped> skipped)
- [ ] Evals pass (if this change touches CLAUDE.md, skills or hooks)
- [ ] No open Important findings

### Tests in this change
| Test file | Seam or behaviour covered |
| --- | --- |

### Findings
| Severity | Axis | Finding | Status |
| --- | --- | --- | --- |

Axis is one of Standards, Spec or Security. Status is Open or Fixed in this run.

### Spec fidelity
<Requirements from the spec that are missing, partial, or exceeded. "All covered" if none.>
```
