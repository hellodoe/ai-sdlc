# AI-Native SDLC

An SDLC for a 10-developer team that combines Anthropic's AI-Native SDLC Playbook (stages, artifacts, human gates, hooks, evals) with Matt Pocock's [skills](https://github.com/mattpocock/skills) (grilling, tracer-bullet tickets, TDD, two-axis code review).

- [docs/ai-native-sdlc-pocock.md](docs/ai-native-sdlc-pocock.md): the full SDLC definition
- [docs/ai-native-sdlc-pocock.pdf](docs/ai-native-sdlc-pocock.pdf): the same, as PDF
- [docs/sdlc-loop.png](docs/sdlc-loop.png): the six-stage loop diagram
- [.claude/skills/to-intent/SKILL.md](.claude/skills/to-intent/SKILL.md): `/to-intent`, which turns a grill-me session into `specs/<slug>/intent.md` and opens a PR for gate G1
- [.claude/skills/review-report/SKILL.md](.claude/skills/review-report/SKILL.md): `/review-report`, which runs typecheck, tests, code-review and security-review and posts the review report on the PR for gate G4
- [.claude/skills/to-release/SKILL.md](.claude/skills/to-release/SKILL.md): `/to-release`, which drafts the release record for a production deploy as a draft GitHub Release for gate G6

![AI-native SDLC loop](docs/sdlc-loop.png)
