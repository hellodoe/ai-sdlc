---
name: to-release
description: Draft the Stage 5 release record for a production deploy (what ships, linked intents and specs, risk, manual steps, rollback plan) as a draft GitHub Release for the release owner. Use when the user says to-release.
disable-model-invocation: true
---

# to-release

Produce the Stage 5 (Deploy) artifact of our SDLC: a release record for one production deploy. The release owner reads it, then approves the production environment. That approval is gate G6.

This skill only drafts. It never deploys, never publishes the release, and never approves an environment.

## Steps

1. **Find the range.** The previous release is the latest published GitHub Release tag (`gh release list`). The range is that tag to `main` (or the commit the user names). Propose the next version using the repo's existing scheme; ask if there is none.
2. **Collect what ships.** For every PR merged in the range (`gh pr list --state merged --search "merged:>DATE"` or the commit log):
   - title and link
   - the intent.md and spec issue it implements, from the PR body or commits
   - the human who approved it (G5)
   - the review-report verdict, from the comment marked `review-report:v1`
   - risk tier label, if any
   Flag any PR with no human approval or no Ready review report. Do not drop it silently.
3. **Find risky changes.** Look for database migrations, config or infra changes, feature flags, new environment variables, and dependency upgrades in the range.
4. **Manual steps.** If any change needs steps a human must run, use the wizard skill to generate a script for them and link it.
5. **Rollback plan.** For each risky change, say how to undo it: revert commit, flag off, down-migration, or "forward-fix only" with the reason.
6. **Write the record** with the template below, keeping it under one page.
7. **Save it** as a draft: `gh release create <version> --draft --target <sha> --title "<version>" --notes-file <file>`. If the draft for this version already exists, update its notes instead.
8. Tell the user the draft link and what the release owner must check before approving the production environment.

## Template

```markdown
## <version> · <date>

Target commit `<sha>` · Previous release <tag>

### What ships
| PR | Intent / spec | Risk tier | G5 approver | Review report |
| --- | --- | --- | --- | --- |

### Needs attention
<PRs missing a human approval or a Ready review report. "None" if empty.>

### Risky changes
| Change | Type | Rollback |
| --- | --- | --- |

Type is one of migration, config, infra, flag, env var, dependency.

### Manual steps
<Link to the wizard script, or "None".>

### G6 checklist for the release owner
- [ ] Every PR above has a human approval and a Ready review report
- [ ] Staging deploy of this commit is healthy
- [ ] Rollback plan is understood for every risky change
- [ ] Manual steps have an owner
```
