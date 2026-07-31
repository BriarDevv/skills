# skills

Personal [Agent Skills](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview)
library for Claude Code — methodology skills usable from any repo, installed
globally as junction links by the `workstation` repo's installer.

Built on the context-engineering standard defined in
[Context-Engineering](https://github.com/BriarDevv/Context-Engineering):
SKILL.md under 500 lines, references one level deep, third-person
descriptions with triggers, and 3 evals per skill that change before the
skill's content does.

## Skills

| Skill | What it does |
|---|---|
| [`reviewing-plans/`](skills/reviewing-plans/) | Adversarial review of plans, specs, and proposals before execution |
| [`tracing-root-causes/`](skills/tracing-root-causes/) | Disciplined causal analysis: competing hypotheses, evidence ranked by strength, active disconfirmation |

## Used alongside (not in this repo)

Day to day this library is complemented by two plugin skill sets, installed
from the official marketplace by the `workstation` installer:

- [`frontend-design`](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/frontend-design)
  (Anthropic) — UI/UX implementation guidance.
- [`superpowers`](https://github.com/obra/superpowers) by Jesse Vincent — TDD,
  debugging, and collaboration workflows (vendored in the
  [official marketplace](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/superpowers)).

## Provenance

Both initial skills were salvaged from oh-my-claudecode's agent pack (see
Context-Engineering ADR-002) and first lived in that repo; they moved here
when the library split from the standard
([ADR-001](docs/adrs/ADR-001-standalone-skills-repo.md)).
